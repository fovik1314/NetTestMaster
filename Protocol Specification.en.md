[中文版](Protocol%20Specification.md)

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/deed.en) license. You are allowed to use, adapt, and distribute this work for free for non-commercial purposes, but you must **clearly attribute the original author**, and **any commercial use is strictly prohibited** (such as sales, advertising, profit-making projects, etc.). Derivative works must also follow the same terms (attribution + non-commercial). Commercial use requires additional authorization. See the LICENSE file for details.

------

# Protocol Comparison and Application Scenarios Explained

## 1. ICMP (Ping Protocol)

### Principle

By sending an **ICMP Echo Request** packet to the target host and receiving an **ICMP Echo Reply** packet, the basic latency and connectivity at the network layer are measured.

### Advantages

- **Simple and fast**: Almost all operating systems and network devices support ICMP.
- **Lightweight**: No need to establish complex connections, very low overhead.
- **Direct to network layer**: Directly reflects the quality of the network path (such as routing latency, packet loss rate).

### Disadvantages

- **Non-business traffic**: Measures "network latency to the server," not "DNS resolution latency."
- **Easily restricted**: Many firewalls or DNS servers actively drop ICMP packets (e.g., Cloudflare blocks ICMP by default).
- **Partial data**: Only reflects the network layer status, cannot represent the real performance of the application layer (DNS).

### Applicable Scenarios

- **Network connectivity check**: Quickly verify if the server is online or the network is reachable.
- **Basic latency measurement**: Roughly assess the quality of the network path (such as cross-region backbone latency).
- **Troubleshooting**: Locate network interruptions or routing anomalies.

## 2. UDP (Standard DNS Query Protocol)

### Principle

Send a standard DNS query request (such as A/AAAA record) to the **UDP 53 port** of the DNS server, and measure the total time from sending the request to receiving the response.

### Advantages

- **Real business simulation**: Directly measures the end-to-end latency of DNS resolution, reflecting the actual user experience.
- **Wide compatibility**: 99% of DNS servers support UDP queries (RFC standard).
- **Efficiency**: The connectionless nature of UDP usually results in lower latency than TCP.

### Disadvantages

- **Port blocking risk**: Some strict network environments (such as enterprise firewalls) may block UDP 53 port.
- **Packet loss ambiguity**: Cannot distinguish whether packet loss is due to network issues or server non-response.
- **Packet size limitation**: UDP message size is limited (usually 512 bytes), not suitable for large DNS responses.

### Applicable Scenarios

- **DNS performance benchmark testing**: Accurately evaluate the resolution speed of DNS servers (such as public DNS comparison).
- **User experience simulation**: Measure response latency in real business scenarios (such as DNS resolution before website access).
- **Routine monitoring**: Long-term tracking of DNS service availability and stability.

## 3. TCP (DNS over TCP/DoT)

### Principle

Establish a connection (three-way handshake) with the DNS server's **53 port** via TCP protocol, send a DNS query, and measure the response time. Some scenarios may involve encryption (such as DoT).

### Advantages

- **Reliability**: TCP's retransmission mechanism ensures the complete transmission of large messages (such as DNSSEC responses).
- **Bypass restrictions**: When UDP 53 port is blocked, TCP can be used as a backup solution.
- **Security**: Supports encrypted protocols such as DoT (DNS over TLS) to prevent data tampering and eavesdropping.

### Disadvantages

- **Higher latency**: The three-way handshake and connection maintenance add extra overhead.
- **Non-mainstream query method**: More than 95% of DNS queries use UDP by default.
- **Testing limitations**: Only measures TCP handshake and port reachability, not DNS resolution efficiency.

### Applicable Scenarios

- **Large message transmission**: Needed to obtain large DNS responses (such as when DNSSEC is enabled).
- **Network restriction bypass**: Used in environments where UDP 53 port is blocked (such as some public Wi-Fi).
- **Security requirements**: Use DoT/DoH encrypted communication in enterprise intranets or privacy-sensitive scenarios.

## Summary and Recommendations

| Protocol | Core Capability           | Recommended Use Case                        |
| -------  | ------------------------ | ------------------------------------------- |
| ICMP     | Network connectivity and basic latency | Quickly troubleshoot network interruptions or routing issues |
| UDP      | Real DNS resolution performance      | Evaluate DNS server response speed and user experience |
| TCP      | Reliability and encrypted communication | Special scenarios (large messages, port blocking, security needs) |

- **Prefer UDP**: If the goal is to measure real DNS resolution latency, UDP is the best choice.
- **ICMP as backup**: Use only for rough network quality data, be aware results may be filtered.
- **TCP for special needs**: Use only when UDP is unavailable, or when encryption or large message transmission is required.

## Recommended Configuration

- protocol_type: "UDP" (and ensure to use standard DNS query packets)
- If UDP is blocked, consider using ICMP or TCP.

## File Description

1. **Structure optimization**: Enhanced readability with tables and section headings, clearly distinguishing principles, pros and cons, and scenarios.
2. **Scenario expansion**: Added practical cases (such as DNSSEC, enterprise firewalls) to help users understand technical details.
3. **Configuration example**: Provided YAML format configuration template for direct practical guidance.
4. **Security supplement**: Emphasized the role of DoT/DoH in privacy protection, in line with modern network security needs.

## Author Information

- **Author**: LanHong
- **Contact**: xyz9010@outlook.com 