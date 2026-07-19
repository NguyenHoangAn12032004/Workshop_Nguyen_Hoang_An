---
title: "Blog 2: Scaling to 121M gRPC Connections"
date: 2026-07-19
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
description: "How bitdrift leveraged Amazon CloudFront and Route 53 to scale telemetry systems to 121 million concurrent gRPC connections during live sporting events."
tags: ["AWS", "Networking", "gRPC", "CloudFront", "Route 53", "Scaling"]
---

# How Bitdrift Scaled to 121 Million Concurrent gRPC Connections on Amazon CloudFront

*This post shares my technical insights and reflections as a student intern in the **AWS First Cloud AI Journey** program, diving into a massive real-world serverless architecture solution.*

---

## 📌 The Journey Begins

Imagine you are watching a live football game with millions of other fans, and your app receives real-time telemetry updates every second. Behind the scenes, the engineering complexity is mind-boggling. When I read about **121 million concurrent gRPC connections** handled on AWS, my jaw dropped. As a cloud student, I have set up simple HTTP servers, but scaling long-lived, bi-directional gRPC connections to over a hundred million concurrent users is an entirely different league. This architecture post from AWS shows how clever routing and CDN designs can conquer extreme traffic peaks.

---

## 🏛️ Original Architecture Deep-Dive

The original post explores how **bitdrift**, a live mobile telemetry platform, designed its infrastructure to ingest telemetry data from millions of mobile devices simultaneously during high-traffic sporting events.

### Context & Challenges
Traditional HTTP APIs are stateless and short-lived. Telemetry systems, however, rely on **gRPC over HTTP/2**, which uses persistent, long-lived TCP connections. 
* **Connection Hotspotting:** If all clients resolve DNS to a single IP address, certain servers get overloaded while others remain idle.
* **DNS Caching Bottlenecks:** Standard DNS caching prevents clients from dynamically redistributing their load.
* **Pre-warming Overhead:** Scaling up traditional Application Load Balancers (ALBs) to handle sudden spikes of 100 million+ connections requires time-consuming pre-warming coordination with AWS support.

### Architectural Layout
Bitdrift resolved these challenges by designing a hybrid edge-to-origin routing pipeline:
* **Amazon CloudFront:** Terminates TLS at the AWS edge, reducing latency for clients worldwide. CloudFront also handles HTTP/2 connection reuse, acting as the front shield.
* **Amazon Route 53 with Multi-Value Answer (MVA) Routing:** Resolves client requests by returning up to 8 randomized IP addresses from a pool of healthy targets, ensuring an even distribution of connections.
* **Network Load Balancers (NLBs):** Handles millions of TCP packets per second and routes them directly to Envoy proxy fleets running on Amazon EKS.
* **Envoy Proxies:** Acts as the internal gRPC router, balancing requests across telemetry storage engines.

![Bitdrift gRPC Telemetry Ingestion Architecture Diagram](/images/3-BlogsPosted/blog2-grpc-scaling.png)

---

## 🛠️ Deep Academic Analysis & Technical Highlights

Understanding how bitdrift achieved this scale requires zooming into the networking protocols:

### 1. HTTP/2 Multiplexing vs. HTTP/1.1 Persistent Connections
In HTTP/1.1, concurrent requests require multiple separate TCP connections, creating severe connection overhead. HTTP/2 introduces **multiplexing**, allowing multiple requests and responses to be sent concurrently over a single TCP connection.
gRPC relies on HTTP/2 streams. However, keeping millions of TCP connections open on backend application servers drains memory resources due to TCP socket buffers. 
Offloading these connections to **Amazon CloudFront** Edge locations shifts the memory load of TLS/TCP state tracking to AWS's global edge network.

### 2. Route 53 Multi-Value Answer (MVA) vs. Round-Robin DNS
Standard round-robin DNS returns a list of IP addresses in a static order. When a client caches the first IP, it sends all traffic there, leading to unbalanced traffic patterns.
Route 53 **Multi-Value Answer (MVA)** routing returns up to 8 randomized, healthy IP addresses from a large pool of resource records. When combined with clientside random selection and a short DNS Time-to-Live (TTL = 60s), clients continuously shuffle their target IPs, resulting in a near-perfect distribution of connections across NLBs without relying on a centralized hardware bottleneck.

$$\text{Load Share per IP} \approx \frac{\text{Total Traffic}}{N_{\text{healthy records}}}$$

### 3. NLB Pass-Through Architecture
Traditional Application Load Balancers (ALBs) operate at Layer 7 (Application), parsing HTTP headers. This consumes significant CPU and memory, requiring pre-warming. Network Load Balancers (NLBs) operate at Layer 4 (Transport), routing raw TCP streams directly to the target EKS nodes. This allows the NLBs to handle millions of connections per second instantaneously.

| Challenge | Solution | Key Learning |
| :--- | :--- | :--- |
| **TLS/TCP handshake storm** | Offloaded TLS termination to **Amazon CloudFront** edge locations. | Terminating SSL closer to the user drastically reduces latency and load on the origin server. |
| **Uneven traffic distribution** | Utilized **Route 53 Multi-Value Answer (MVA)** returning 8 randomized IP addresses. | MVA routing is a simple but highly effective way to achieve clientside load balancing without a single point of failure. |
| **Scaling load balancers** | Used **Network Load Balancers (NLBs)** instead of ALBs. | NLBs route TCP traffic directly without looking at HTTP layers, allowing them to handle millions of requests instantly with no pre-warming. |
| **Persistent connection stickiness** | Configured Envoy to gracefully terminate connections after a maximum lifetime. | Recycling long-lived connections prevents single nodes from becoming permanent hotspots. |

---

## 💡 Reflection & Internship Lessons

As a cloud student, this architecture taught me how critical the networking layer is to system scaling.

Here are my major takeaways:
1. **CDNs are not just for static files:** Before reading this, I thought CloudFront was only for caching images and React scripts. Learning that CloudFront terminates TLS and proxies gRPC traffic at the edge was a revelation!
2. **DNS as a Load Balancer:** Route 53's Multi-Value Answer is a brilliant mechanism. Instead of relying on a centralized load balancer that can bottleneck, we can let Route 53 distribute IPs and let clients load balance themselves.
3. **The Importance of Connection Recycling:** In school, we are taught to keep connections alive for performance. But in massive distributed systems, keeping connections alive *forever* leads to load imbalance. Learning to periodically recycle connections is a counter-intuitive but essential lesson!

Seeing this architecture succeed makes me incredibly eager to experiment with Route 53 and CloudFront in my own labs. It is a masterclass in elegant network design!

---

## 🔗 References
* [Original AWS Architecture Blog: How bitdrift scaled to 121 million concurrent gRPC connections](https://aws.amazon.com/blogs/architecture/how-bitdrift-scaled-to-121-million-concurrent-grpc-connections-on-amazon-cloudfront-for-live-telemetry-sporting-events/)
* [Amazon Route 53 Developer Guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome.html)