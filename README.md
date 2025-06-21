[English Version](README.en.md)

本作品采用 [知识共享署名-非商业性使用 4.0 国际 (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/deed.zh) 协议授权。允许他人免费非商业性使用、改编和传播作品，但必须**清晰标注原作者署名**，且**禁止任何商业用途**（如销售、广告、盈利项目等），修改后的衍生作品也需遵循相同条款（署名+非商用），商业使用需额外获得授权。详见LICENSE文件。

------

# NetTestMaster - 通用网络协议批量测试工具

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-EF9421.svg?logo=creative-commons&logoColor=white)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Python](https://img.shields.io/badge/Python-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![IP Stack](https://img.shields.io/badge/IP_Stack-IPv4%20|%20IPv6-0066CC.svg?logo=cloudflare&logoColor=white)](https://en.wikipedia.org/wiki/IPv6)
![](https://img.shields.io/badge/OS-Windows%7CLinux%7CmacOS-green)
[![Network Protocols](https://img.shields.io/badge/Protocols-ICMP%20|%20UDP%20|%20TCP%20|%20DNS-8A2BE2.svg?logo=icloud&logoColor=white)](https://en.wikipedia.org/wiki/Internet_protocol_suite)

一款支持多协议（ICMP/UDP/TCP/DNS）的网络测试工具，提供高效并发测试、终端表格输出和Excel自动导出功能，适用于网络运维、性能评估及科研场景。

<details>
<summary>Demo</summary>

![](https://github.com/fovik1314/NetTestMaster/blob/0ec535a90917fcbac4eac128f19705ff34761ca5/Demo/Demo.png)

</details>

<details>
<summary>目录</summary>

1. [Demo](#Demo)
2. [目录](#目录)
3. [脚本简介](#脚本简介)
4. [环境准备](#环境准备)
5. [脚本结构说明](#脚本结构说明)
6. [脚本参数详解](#脚本参数详解)
7. [如何运行脚本](#如何运行脚本)
8. [结果解读](#结果解读)
9. [进阶用法与扩展建议](#进阶用法与扩展建议)
10. [脚本结构简要说明](#脚本结构简要说明)
11. [常见问题与解决](#常见问题与解决)
12. [联系方式与反馈](#联系方式与反馈)

</details>

<details>
<summary>脚本简介</summary>
`NetTestMaster` 是一款支持多协议（ICMP/UDP/TCP/DNS）、多线程并发、断点续传、自动导出 Excel、终端美观表格输出的网络测速工具。它主要用于**批量测试**各大公共 DNS 服务器（或自定义目标）的网络连通性、延迟、丢包率、协议兼容性等，支持IPv4/IPv6，支持终端美观表格输出和Excel自动导出，适合网络运维、教育、科研等场景。

---

</details>

<details>
<summary>环境准备</summary>

### 1. Python 环境

- 操作系统：Windows、Linux、macOS均可。
- 推荐 Python 3.7 及以上版本。
- Windows 用户建议以"管理员身份"运行（ICMP 协议需要）。

### 2. 必需依赖库

脚本依赖以下第三方库：

- ping3
- pandas
- wcwidth
- dnspython
- openpyxl

### 安装命令（推荐在命令行/终端中执行）：

可执行脚本实现一键自动安装

```bash
pip install ping3 pandas wcwidth dnspython openpyxl
```

如遇网络问题可用清华镜像：

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple ping3 pandas wcwidth dnspython openpyxl
```

### 脚本结构说明

- **依赖导入区**：所有用到的库
- **工具函数区**：如获取桌面路径、时间戳、协议包大小等
- **配置参数区**：所有可调参数集中管理
- **Excel导出格式区**：表头、列宽、对齐方式等
- **DNS与国家映射表**：常用DNS及归属地
- **断点续传区**：测速中断恢复
- **域名解析区**：本地/指定DNS解析
- **测速与调度区**：核心测速逻辑
- **表格格式区**：终端输出美化
- **主函数**：主流程、表格输出、Excel导出
- **程序入口**：直接运行时自动执行

</details>

<details>
<summary>脚本参数详解</summary>

所有参数集中在 `config` 字典中，支持灵活修改。主要参数如下：

### 1. 基础参数

| 参数名             | 说明                                              | 示例/默认值 |
| ------------------ | ------------------------------------------------- | ----------- |
| total_scan_time    | 总扫描时间限制（秒），超时强制终止                | 60          |
| concurrent_threads | 并发线程数，越大速度越快，建议 50~200 之间        | 100         |
| timeout            | 单次请求超时时间（秒）                            | 0.5         |
| protocol_type      | 测试协议类型：ICMP、UDP、TCP                      | "UDP"       |
| enable_ipv6        | 是否启用 IPv6 测试                                | False       |
| use_local_ip       | 是否显示本地出口 IP                               | True        |
| resolved_ip_type   | 解析 IP 类型：'A'（IPv4）、'AAAA'（IPv6）、'auto' | "A"         |

### 2. 测试目标参数

| 参数名             | 说明                                    | 示例/默认值     |
| ------------------ | --------------------------------------- | --------------- |
| test_count_per_dns | 每个目标测试次数                        | 3               |
| test_domain        | 指定测试域名（留空则按协议类型测试 IP） | "www.baidu.com" |
| total_test_count   | 限制总测试次数（None 不限制）           | None            |

### 3. 导出参数

| 参数名               | 说明                     | 示例/默认值 |
| -------------------- | ------------------------ | ----------- |
| export_to_desktop    | 是否导出到桌面           | True        |
| export_to_script_dir | 是否导出到脚本同路径下   | False       |
| export_path          | 兜底导出路径（自动生成） | 自动生成    |

### 4. 显示与导出字段参数

| 参数名           | 说明                    | 示例/默认值 |
| ---------------- | ----------------------- | ----------- |
| top_n            | 显示/导出前 N 个结果    | 30          |
| show_config      | 是否输出配置区内容      | True        |
| show_recv_sent   | 是否输出 Recv/Sent 数据 | True        |
| show_loss_rate   | 是否输出丢包率          | True        |
| show_protocol    | 是否输出协议类型        | True        |
| show_country     | 是否输出国家            | True        |
| show_packet_size | 是否输出测试包大小      | True        |

### 5. 断点续传参数

| 参数名        | 说明                 | 示例/默认值        |
| ------------- | -------------------- | ------------------ |
| enable_resume | 是否启用断点续传功能 | False              |
| resume_file   | 断点续传记录文件     | "scan_resume.json" |

### 6. 协议测试数据包大小参数

| 参数名      | 说明                                                         | 示例/默认值 |
| ----------- | ------------------------------------------------------------ | ----------- |
| packet_size | 测试包大小（字节）：int 为自定义，"default" 推荐，"auto" 动态 | "auto"      |

---

</details>

<details>
<summary>如何运行脚本</summary>

### 1. 修改参数

- 用文本编辑器（如 VSCode、Notepad++）打开 `NetTestMaster.py`。
- 找到 `config = { ... }` 区块，根据需求修改参数。

### 2. 运行脚本

#### Windows

- 以管理员身份打开命令提示符（cmd）或 PowerShell。
- 切换到脚本所在目录：

```bash
cd 路径\到\NetTestMaster.py\所在文件夹
```

- 运行脚本：

```bash
python NetTestMaster.py
```

#### Linux/Mac

- 打开终端，切换到脚本目录，运行：

```bash
python3 NetTestMaster.py
```

---

</details>

<details>
<summary>结果解读</summary>

### 1. 终端输出

- 会以美观表格形式输出每个 DNS 的延迟、丢包率、协议、国家等信息。
- 支持自适应宽度、对齐、超长 IP 折叠显示。
- 末尾会显示总用时、导出路径等信息。

### 2. Excel 导出

- 默认导出到桌面（可配置）。
- 文件名如 `NetTest_20240601_153000.xlsx`，包含所有表格字段。
- 表头、内容自动美化，丢包率为百分比格式，便于后续分析。

### 3. 断点续传

- 启用 `enable_resume` 后，测速中断可自动恢复进度。
- 断点文件为 `scan_resume.json`，参数变动会自动清理。

</details>

<details>
<summary>进阶用法与扩展建议</summary>

### 1. 自定义目标

- 默认测试内置 DNS 列表。
- 如需自定义目标，可修改 `DNS_NAMES` 字典，增删目标IP及名称，或扩展 `addresses` 生成逻辑。

### 2. 测试域名

- 设置 `test_domain` 后，强制使用 DNS 查询协议，统计各 DNS 解析该域名的速度。

### 3. IPv6 支持

- 设置 `enable_ipv6=True`，可测试 IPv6 DNS。
- 需保证本机有 IPv6 网络环境。

### 4. 导出多路径

- 同时设置 `export_to_desktop` 和 `export_to_script_dir`，会导出到桌面和脚本目录。

### 5. 支持批量导入目标

- 可扩展为从文件读取目标列表（如CSV、TXT）。

### 6. 命令行参数支持

- 可用 `argparse` 增加命令行参数，动态指定配置文件、目标、导出路径等。

### 7. 智能包大小

- 可在 `get_packet_size` 或 `test_ping` 中根据丢包率、延迟等实时调整包大小。

### 8. 日志与调试

- 可引入 `logging` 替代 `print`，支持多级别日志和文件输出。

---

</details>

<details>
<summary>脚本结构简要说明</summary>

- **工具函数区**：如获取桌面路径、时间戳、IP 判断等。
- **配置参数区**：所有可调参数集中管理。
- **Excel 导出格式区**：统一设置表头、列宽、对齐方式。
- **DNS 列表与国家映射**：内置常用 DNS 及归属地。
- **断点续传区**：支持测速中断恢复。
- **域名解析区**：支持本地和指定 DNS 解析。
- **核心测速区**：多协议测速、并发调度、统计。
- **表格输出区**：终端美观表格输出。
- **主函数**：主流程调度、排序、导出。
- **程序入口**：直接运行时自动执行测速。

---

</details>

<details>
<summary>常见问题与解决</summary>

- **依赖未安装**：请检查依赖是否全部安装。
- **ICMP 权限不足**：请以管理员身份运行。或切换为UDP/TCP协议。
- **Excel 打不开/乱码**：请确保 Excel 支持 UTF-8，或更换字体。
- **IPv6 无法测速**：请确认本机 IPv6 网络正常。
- **网络不通时/超时**：检查本地网络、目标DNS可达性，适当增大 `timeout`。
- **断点续传无效**：请确认本机 IPv6 网络正常。需将 `enable_resume` 设为True，且不要手动删除 `resume_file`。

---

</details>

<details>
<summary>联系方式与反馈</summary>

- **作者**：兰宏（LanHong）
- **联系方式**：xyz9010@outlook.com

---

**祝你使用愉快！如需进一步定制或遇到疑难，欢迎随时提问！**

</details>
