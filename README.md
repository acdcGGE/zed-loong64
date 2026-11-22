# Zed 简体中文版（1.31.0）

基于 [Zed](https://zed.dev) 1.31.0，汉化词条来自 [ReachForStar/zed-ZH_CN](https://github.com/ReachForStar/zed-ZH_CN)，覆盖 70 个模块、4060 条词条（设置页、命令面板、项目面板、Git 面板、欢迎页、智能体等）。

## 安装

```bash
sudo pacman -U zed-zhcn-1.31.0-1-x86_64.pkg.tar.zst   # 或 ...-loong64...
```

Debian / Ubuntu：

```bash
sudo apt install ./zed-zhcn_1.31.0-1_amd64.deb        # loong64 版改文件名
```

包名 `zed-zhcn`，`provides: zed`，与 `zed`、`zed-loc`、`zed-editor` 冲突。

另附未汉化原版 loong64 包：`zed-1.31.0-1-loong64.pkg.tar.zst`、`zed_1.31.0-1_loong64.deb`（包名 `zed`），与汉化版互斥，二者择一安装。

## 两个包的区别

| 项目 | x86_64 | loong64 |
| --- | --- | --- |
| 构建方式 | 本机原生编译 | 交叉编译（`loongarch64-linux-gnu`）|
| 协作 / 语音通话 | 真实 libwebrtc，可用 | mock 实现，不可用 |
| 崩溃上报（minidumper）| 启用 | 禁用（no-op）|
| 沙箱 seccomp | 原生 | 补 `AUDIT_ARCH` 适配 |
| zed-editor 主程序 | 314 MiB | 279 MiB |
| 安装包 | 115 MiB | 101 MiB |
| 汉化范围 | 完全一致 | 完全一致 |

两者功能差异仅源于上游依赖（libwebrtc、minidumper）不支持 LoongArch；日常编辑、LSP、Git、终端、AI 面板均完整可用。

## 从源码构建

在 Arch Linux（对应架构）仓库根目录：

```bash
makepkg -f
```

词条位于 `crates/zed_i18n/locales/`，修改后重新编译 `zed_i18n` 即可生效（`build.rs` 已声明变更监听）。
