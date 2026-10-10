# RyazhTune

RyazhTune 是 Nintendo Switch 的第三方后台音乐模块，允许用户在游戏和系统导航过程中播放自己的音频文件。本项目是原版 sys-tune 的 fork，扩展了额外功能以改善用户体验和配置管理。

[![最新版本](https://img.shields.io/github/v/release/Dimasick-git/RyazhaTune?display_name=tag&label=%E7%89%88%E6%9C%AC&style=for-the-badge)](https://github.com/Dimasick-git/RyazhaTune/releases/latest)
[![下载量](https://img.shields.io/github/downloads/Dimasick-git/RyazhaTune/total?label=%E4%B8%8B%E8%BD%BD&style=for-the-badge)](https://github.com/Dimasick-git/RyazhaTune/releases)
![README 访问量](https://visitor-badge.laobi.icu/badge?page_id=Dimasick-git.RyazhaTune&left_text=%E8%AE%BF%E9%97%AE%E9%87%8F)

> 下载计数器获取的是 GitHub Releases 的聚合数据；访问计数器由外部 badge 服务提供，用于展示公开统计信息。

## 功能特性

- **持久播放列表**：播放列表状态在系统重启后仍然保留。
- **自动启动功能**：可配置的系统启动时自动初始化播放。
- **按 Title ID 过滤模式**：`Normal`（普通）/ `Whitelist`（白名单）/ `Blacklist`（黑名单），可在 overlay 中按游戏单独设置。
- **灵活的焦点策略**：全局 `Title Focus`（游戏焦点）+ 按游戏 `Custom Focus`（自定义焦点：播放 / 暂停 / 跳过），并包含 HOME 菜单的独立行为。
- **多语言界面**：支持多种语言区域，并提供独立语言选择界面（双栏布局）。
- **音频格式支持**：内置解码 MP3、FLAC、WAV 和 WAVE 格式。
- **Overlay 集成**：通过 Tesla overlay 界面无缝控制。
- **5 段均衡器**：自研实现，统一 live-tuner 由五个垂直推子组成（100 Hz、300 Hz、1 kHz、3 kHz 和 10 kHz），即时生效并内置预设配置。推子界面 `A` 开启编辑，`L/R` 选择频段，`↑/↓` 调整增益，`A` 或 `B` 返回列表控制；也支持垂直触摸调节。输出目标可切换为 RyazhTune 音乐（软件 DSP）或游戏/系统（ALC5639 模拟输出的硬件 EQ；底座 HDMI 输出不支持）。

## 安装方法

1. 从仓库 Release 页面下载最新版本的压缩包（`.zip`）。
2. 将压缩包内容解压到 Nintendo Switch SD 卡根目录。
3. 将受支持的音频文件（MP3、FLAC、WAV）放入 SD 卡。
4. 打开 Tesla overlay 菜单即可控制播放与配置。

## 架构与组件

项目由以下几个核心组件构成：

- **sys-tune**：后台系统模块（sysmodule），负责通过 `audren` 服务进行音频解码与播放。
- **overlay**：Tesla overlay 插件（`.ovl`），提供图形用户界面用于控制播放。
- **ipc**：进程间通信定义，用于 overlay 与系统模块之间的交互。
- **libtesla**：内置 UI 库，用于渲染 overlay 界面。
- **common**：通用工具，包括配置管理（`minIni`）、SD 卡访问与进程管理。

## 配置说明

配置文件在首次使用时自动生成，存放于 `/config/RyazhTune/`。模块使用 `minIni` 解析配置。

- `config.ini`：主要配置参数（自动启动、音量、随机播放、重复模式、语言、按 Title ID 过滤以及 `equalizer` 小节）。
- `config.ini` 中的 `whitelist` / `blacklist` 小节：用于 Whitelist/Blacklist 模式的按游戏标志。
- `saved_playlist.txt`：持久播放列表的状态。
- `play_source.txt`：上次的播放来源（`Playlist` 或 `Folder:<路径>`）。

### 过滤行为（TuneMode）

- `Normal`（普通）：音乐不按 Title ID 过滤。
- `Whitelist`（白名单）：仅允许播放 `whitelist` 中已启用 Title ID 的音乐。
- `Blacklist`（黑名单）：禁止播放 `blacklist` 中已启用 Title ID 的音乐。

## 从源码编译

### 前置要求

- [devkitPro](https://devkitpro.org/) 并安装 `switch-dev` 软件包组。
- 安装 GNU Make。
- 系统环境中可用 `python3` 命令。
- 确保环境变量 `DEVKITPRO` 已正确设置。

### 标准编译

编译项目核心组件（系统模块和 overlay）：

```bash
make all
```

生成带安装目录结构的压缩包：

```bash
make dist
```

### 高级编译（Horizon-OC）

使用 Horizon-OC 变更集成并修补 Atmosphere 的编译方式：

1. 运行自动配置脚本：
   ```bash
   chmod +x setup_build_env.sh
   ./setup_build_env.sh
   ```
2. 脚本会自动克隆所需仓库（Atmosphere、Horizon-OC），应用 `ldr_process_creation.cpp` 补丁并启动编译流程。

## 致谢

本项目基于原版 sys-tune 开发者的基础工作。感谢原仓库的作者以及整个 Nintendo Switch homebrew 社区。

## 版本信息

- **当前版本：** <!-- CURRENT_VERSION_START -->5.6.1<!-- CURRENT_VERSION_END -->
- **状态：** 稳定版

## TODO

- 支持多个播放列表（创建 / 切换 / 保存独立的曲目集合）。

## 许可证

本项目以 GNU General Public License Version 2（GPLv2）许可证分发。完整信息见 `LICENSE` 文件。内置组件 `libtesla` 同样以 GPLv2 许可证分发。
