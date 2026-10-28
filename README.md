# Learning Notes / 学习笔记

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

Knowledge & study notes distilled from tech videos (Bilibili / YouTube), summarized with AI assistance and manually reviewed for technical accuracy before publishing.

从 Bilibili / YouTube 技术视频整理的知识性学习笔记：AI 辅助总结 + 人工审校，完全开源。

## Contents / 目录

| Note 笔记 | Topic 主题 | Highlights 要点 |
| :--- | :--- | :--- |
| [计算机网络速通](./计算机网络速通.md) | Computer Networking | Five-layer model, encapsulation, sliding-window protocols (SW/GBN/SR), CRC & Hamming, CSMA/CD, IP addressing & CIDR, routing, TCP congestion control, application protocols; all worked examples re-verified |
| [操作系统速通](./操作系统速通.md) | Operating Systems | Process states & scheduling, synchronization & banker's algorithm, memory management & paging, page replacement, file systems (inode, FAT, indexing), disk scheduling; all worked examples re-verified |
| [Git + GitHub](./Git%20+%20GitHub.md) | Git & GitHub | Working tree / staging / repository model, restore vs reset vs revert, branching, merge & rebase, stash, worktree, SSH authentication, remote workflow |
| [HTTP 状态码](./HTTP%20状态码.md) | HTTP Status Codes | Full 1xx–5xx reference with side-by-side comparisons: 301/302/307/308, 400/422, 401/403, 404/410, 502/503/504 |
| [网络协议体系](./网络协议体系.md) | Network Protocol Landscape | IP, TCP vs UDP, DNS & DoH, HTTP/1.1 → HTTP/3 (QUIC), HTTPS/TLS handshake, MQTT publish-subscribe, WebSocket |
| [CPU架构详解](./CPU架构详解.md) | CPU Architectures | x86 vs ARM vs RISC-V, Apple Silicon SoC & unified memory, GPU/NPU, FPGA, superconducting quantum computing |
| [分区类型详解](./分区类型详解.md) | Disk Partition Types | MBR vs GPT, BIOS vs UEFI boot flow, EFI system partition, MSR, Recovery, Swap, primary/extended/logical partitions |

## How these notes are made

1. Each note starts as an AI-assisted summary of an educational video, organized around the video's structure and worked examples.
2. Before publishing, every technical claim and every worked example (calculations, formulas, command behavior) is re-checked by independent review passes.
3. Notes aim to be accurate revision material, not full-textbook replacements — keep the original courses as your primary source.

## Sources & attribution

- Most source videos were watched on Bilibili and their links were not recorded at the time; where a source is known, it is credited at the end of the individual note (e.g. [网络协议体系](./网络协议体系.md) credits Techs Explained's *Every Network Protocol Explained in 12 minutes* on YouTube).
- Original video content, course names, and trademarks belong to their respective creators. These notes are independent study summaries and do not reproduce video transcripts.
- If you are a content owner and want a note removed, or a source added/corrected, please [open an issue](https://github.com/AkiroMusic/learning-notes/issues).

## License

All notes and documentation in this repository are released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) — free to share and adapt with attribution. See [LICENSE](./LICENSE) for the full text.

---

## 中文说明

这是一份知识性学习笔记合集：基于 Bilibili / YouTube 上的公开技术课程视频，用 AI 辅助总结成文，发布前经人工审校，全部例题与计算逐条重算复核。

- **内容**：计算机网络、操作系统、Git/GitHub、HTTP 状态码、网络协议体系、CPU 架构、磁盘分区，共 7 篇，目录见上表。
- **定位**：复习提纲式的速查笔记，帮助你快速回顾视频课的核心结论与经典例题；请以原课程作为主要学习来源。
- **来源与署名**：多数视频为 B 站课程，原始链接未留存；已知出处的视频在对应笔记末尾标注。如你是原作者，需要删除笔记或补充署名，请提 Issue。
- **许可证**：笔记内容以 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 开源——可自由转载、修改，需注明出处。
