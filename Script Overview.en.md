[中文版](LICENSE.md)

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/deed.en) license. You are allowed to use, adapt, and distribute this work for free for non-commercial purposes, but you must **clearly attribute the original author**, and **any commercial use is strictly prohibited** (such as sales, advertising, profit-making projects, etc.). Derivative works must also follow the same terms (attribution + non-commercial). Commercial use requires additional authorization. See the LICENSE file for details.

------

# Script Overview (General Network Protocol Batch Testing Tool)

`NetTestMaster` is a general-purpose network protocol batch testing tool, suitable for automated connectivity, performance, and quality assessment of a large number of network targets (such as IPs, domains, servers, etc.). The script supports multiple mainstream network protocols (ICMP, UDP, TCP, DNS) and allows flexible configuration of test parameters to adapt to various network environments and testing needs.

Users can customize test protocols, targets, packet size, concurrency, timeout, number of tests, etc., through the configuration section. The script automatically executes tests concurrently, collecting key metrics such as latency, packet loss rate, sent/received packets, and protocol type for each target. Test results are output in a visually appealing table in the terminal and can be exported to a formatted Excel file with one click for archiving and further analysis.

This tool is not only suitable for DNS servers but also for batch health checks, performance comparisons, and network quality monitoring of any network host or service port. It supports IPv4/IPv6 dual stack, breakpoint resume, and custom targets, making it suitable for network operations, education, enterprise intranets, research, and more.

**Main Features:**

- Supports batch testing of multiple protocols: ICMP, UDP, TCP, DNS
- Flexible targets: IP, domain, port, etc.
- Centralized configuration, rich parameters, easy customization
- High concurrency, suitable for large-scale batch testing
- Beautiful terminal table output, automatic beautification for Excel export
- Supports breakpoint resume, IPv6, custom/dynamic packet size
- Suitable for network health checks, performance evaluation, comparative analysis, and more

## Author Information

- **Author**: LanHong
- **Contact**: xyz9010@outlook.com 