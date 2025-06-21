[中文版](README.md)

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/deed.en) license. You are allowed to use, adapt, and distribute this work for free for non-commercial purposes, but you must **clearly attribute the original author**, and **any commercial use is strictly prohibited** (such as sales, advertising, profit-making projects, etc.). Derivative works must also follow the same terms (attribution + non-commercial). Commercial use requires additional authorization. See the LICENSE file for details.NetTestMaster - General Network Protocol Batch Testing Tool

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-EF9421.svg?logo=creative-commons&logoColor=white)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Python](https://img.shields.io/badge/Python-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![IP Stack](https://img.shields.io/badge/IP_Stack-IPv4%20|%20IPv6-0066CC.svg?logo=cloudflare&logoColor=white)](https://en.wikipedia.org/wiki/IPv6)
![](https://img.shields.io/badge/OS-Windows%7CLinux%7CmacOS-green)
[![Network Protocols](https://img.shields.io/badge/Protocols-ICMP%20|%20UDP%20|%20TCP%20|%20DNS-8A2BE2.svg?logo=icloud&logoColor=white)](https://en.wikipedia.org/wiki/Internet_protocol_suite)

A network testing tool supporting multiple protocols (ICMP/UDP/TCP/DNS), providing efficient concurrent testing, terminal table output, and Excel automatic export functions. Suitable for network operation and maintenance, performance evaluation, and scientific research scenarios.

<details>
<summary>Demo</summary>

![](https://github.com/fovik1314/NetTestMaster/blob/0ec535a90917fcbac4eac128f19705ff34761ca5/Demo/Demo.png)

</details>

<details>
<summary>Contents</summary>

1. [Demo](#Demo)
2. [Contents](#Contents)
3. [Script Introduction](#Script-Introduction)
4. [Environment Preparation](#Environment-Preparation)
5. [Script Structure Description](#Script-Structure-Description)
6. [Script Parameter Details](#Script-Parameter-Details)
7. [How to Run the Script](#How-to-Run-the-Script)
8. [Result Interpretation](#Result-Interpretation)
9. [Advanced Usage and Extension Suggestions](#Advanced-Usage-and-Extension-Suggestions)
10. [Brief Script Structure Description](#Brief-Script-Structure-Description)
11. [FAQ and Solutions](#FAQ-and-Solutions)
12. [Contact and Feedback](#Contact-and-Feedback)

</details>

<details>
<summary>Script Introduction</summary>

`NetTestMaster` is a network speed testing tool that supports multiple protocols (ICMP/UDP/TCP/DNS), multi-threaded concurrency, breakpoint resume, automatic Excel export, and beautiful terminal table output. It is mainly used for batch testing the network connectivity, latency, packet loss rate, protocol compatibility, etc. of major public DNS servers (or custom targets). It supports IPv4/IPv6, beautiful terminal table output, and automatic Excel export, suitable for network operation and maintenance, education, scientific research, and other scenarios.

---

</details>

<details>
<summary>Environment Preparation</summary>

### 1. Python Environment

- Operating systems: Windows, Linux, macOS are all supported.
- Python 3.7 or above is recommended.
- Windows users are advised to run as "Administrator" (required for ICMP protocol).

### 2. Required Dependencies

The script depends on the following third-party libraries:

- ping3
- pandas
- wcwidth
- dnspython
- openpyxl

### Installation command (recommended to execute in command line/terminal):

You can execute the script for one-click automatic installation

```bash
pip install ping3 pandas wcwidth dnspython openpyxl
```

If you encounter network issues, you can use the Tsinghua mirror:

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple ping3 pandas wcwidth dnspython openpyxl
```

### Script Structure Description

- **Dependency Import Section**: All used libraries
- **Utility Function Section**: Such as getting desktop path, timestamp, protocol packet size, etc.
- **Configuration Parameter Section**: Centralized management of all adjustable parameters
- **Excel Export Format Section**: Table headers, column widths, alignment, etc.
- **DNS and Country Mapping Table**: Common DNS and attribution
- **Breakpoint Resume Section**: Speed test interruption recovery
- **Domain Name Resolution Section**: Local/specified DNS resolution
- **Speed Test and Scheduling Section**: Core speed test logic
- **Table Format Section**: Terminal output beautification
- **Main Function**: Main process, table output, Excel export
- **Program Entry**: Automatically executed when run directly

</details>

<details>
<summary>Script Parameter Details</summary>

All parameters are centralized in the `config` dictionary and can be flexibly modified. The main parameters are as follows:

### 1. Basic Parameters

| Parameter Name      | Description                                      | Example/Default |
| -------------------| ------------------------------------------------ | -------------- |
| total_scan_time    | Total scan time limit (seconds), forced termination on timeout | 60             |
| concurrent_threads | Number of concurrent threads, the larger the faster, recommended between 50~200 | 100            |
| timeout            | Timeout for a single request (seconds)           | 0.5            |
| protocol_type      | Test protocol type: ICMP, UDP, TCP               | "UDP"          |
| enable_ipv6        | Whether to enable IPv6 testing                   | False          |
| use_local_ip       | Whether to display local outbound IP             | True           |
| resolved_ip_type   | IP type to resolve: 'A' (IPv4), 'AAAA' (IPv6), 'auto' | "A"        |

### 2. Test Target Parameters

| Parameter Name      | Description                                      | Example/Default |
| -------------------| ------------------------------------------------ | -------------- |
| test_count_per_dns | Number of tests per target                       | 3              |
| test_domain        | Specify test domain (leave blank to test IP by protocol type) | "www.baidu.com" |
| total_test_count   | Limit total number of tests (None for unlimited) | None           |

### 3. Export Parameters

| Parameter Name         | Description                                    | Example/Default |
| ----------------------| ---------------------------------------------- | -------------- |
| export_to_desktop     | Whether to export to desktop                   | True           |
| export_to_script_dir  | Whether to export to the script directory      | False          |
| export_path           | Fallback export path (automatically generated) | Auto-generated |

### 4. Display and Export Field Parameters

| Parameter Name      | Description                                      | Example/Default |
| -------------------| ------------------------------------------------ | -------------- |
| top_n              | Show/export top N results                        | 30             |
| show_config        | Whether to output configuration section content   | True           |
| show_recv_sent     | Whether to output Recv/Sent data                 | True           |
| show_loss_rate     | Whether to output packet loss rate               | True           |
| show_protocol      | Whether to output protocol type                  | True           |
| show_country       | Whether to output country                        | True           |
| show_packet_size   | Whether to output test packet size               | True           |

### 5. Breakpoint Resume Parameters

| Parameter Name      | Description                                      | Example/Default |
| -------------------| ------------------------------------------------ | -------------- |
| enable_resume      | Whether to enable breakpoint resume function      | False          |
| resume_file        | Breakpoint resume record file                     | "scan_resume.json" |

### 6. Protocol Test Packet Size Parameters

| Parameter Name      | Description                                      | Example/Default |
| -------------------| ------------------------------------------------ | -------------- |
| packet_size        | Test packet size (bytes): int for custom, "default" recommended, "auto" dynamic | "auto" |

---

</details>

<details>
<summary>How to Run the Script</summary>

### 1. Modify Parameters

- Open `NetTestMaster.py` with a text editor (such as VSCode, Notepad++).
- Find the `config = { ... }` block and modify the parameters as needed.

### 2. Run the Script

#### Windows

- Open Command Prompt (cmd) or PowerShell as administrator.
- Switch to the script directory:

```bash
cd path\to\NetTestMaster.py\folder
```

- Run the script:

```bash
python NetTestMaster.py
```

#### Linux/Mac

- Open the terminal, switch to the script directory, and run:

```bash
python3 NetTestMaster.py
```

---

</details>

<details>
<summary>Result Interpretation</summary>

### 1. Terminal Output

- Outputs each DNS's latency, packet loss rate, protocol, country, etc. in a beautiful table format.
- Supports adaptive width, alignment, and folding display for long IPs.
- The end will display total time used, export path, and other information.

### 2. Excel Export

- By default, exports to the desktop (configurable).
- File name like `NetTest_20240601_153000.xlsx`, containing all table fields.
- Table headers and content are automatically beautified, packet loss rate is in percentage format for easy analysis.

### 3. Breakpoint Resume

- After enabling `enable_resume`, speed test interruptions can automatically resume progress.
- Breakpoint file is `scan_resume.json`, parameter changes will automatically clear it.

</details>

<details>
<summary>Advanced Usage and Extension Suggestions</summary>

### 1. Custom Targets

- By default, tests the built-in DNS list.
- To customize targets, modify the `DNS_NAMES` dictionary to add or remove target IPs and names, or extend the `addresses` generation logic.

### 2. Test Domain

- After setting `test_domain`, DNS query protocol is forcibly used, and the speed of each DNS resolving the domain is counted.

### 3. IPv6 Support

- Set `enable_ipv6=True` to test IPv6 DNS.
- Ensure the local machine has an IPv6 network environment.

### 4. Multi-path Export

- Setting both `export_to_desktop` and `export_to_script_dir` will export to both the desktop and the script directory.

### 5. Support for Batch Import of Targets

- Can be extended to read target lists from files (such as CSV, TXT).

### 6. Command Line Parameter Support

- Use `argparse` to add command line parameters for dynamically specifying configuration files, targets, export paths, etc.

### 7. Intelligent Packet Size

- In `get_packet_size` or `test_ping`, dynamically adjust packet size based on packet loss rate, latency, etc.

### 8. Logging and Debugging

- Introduce `logging` to replace `print`, supporting multi-level logging and file output.

---

</details>

<details>
<summary>Brief Script Structure Description</summary>

- **Utility Function Section**: Such as getting desktop path, timestamp, IP judgment, etc.
- **Configuration Parameter Section**: Centralized management of all adjustable parameters.
- **Excel Export Format Section**: Unified settings for table headers, column widths, alignment, etc.
- **DNS List and Country Mapping**: Built-in common DNS and attribution.
- **Breakpoint Resume Section**: Supports speed test interruption recovery.
- **Domain Name Resolution Section**: Supports local and specified DNS resolution.
- **Core Speed Test Section**: Multi-protocol speed test, concurrent scheduling, statistics.
- **Table Output Section**: Beautiful terminal table output.
- **Main Function**: Main process scheduling, sorting, export.
- **Program Entry**: Automatically executes speed test when run directly.

---

</details>

<details>
<summary>FAQ and Solutions</summary>

- **Dependency not installed**: Please check if all dependencies are installed.
- **ICMP insufficient permissions**: Please run as administrator, or switch to TCP/UDP protocol.
- **Excel cannot be opened/garbled**: Please ensure Excel supports UTF-8, or change the font.
- **IPv6 cannot be tested**: Please confirm the local IPv6 network is normal.
- **Network unreachable/timeout**: Check local network and target DNS reachability, and appropriately increase `timeout`.
- **Breakpoint resume invalid**: Please confirm the local IPv6 network is normal. Set `enable_resume` to True, and do not manually delete `resume_file`.

---

</details>

<details>
<summary>Contact and Feedback</summary>

- **Author**: LanHong
- **Contact**: xyz9010@outlook.com

---

**Wish you a pleasant experience! If you need further customization or encounter any problems, feel free to ask!**

</details> 
