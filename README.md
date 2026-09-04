# dsh-reveal-explorer

**会话头部一个按钮，用系统文件管理器打开当前工作区目录。**
**One button in the session header to open the current workspace in your system file manager.**

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（下称 dsh）的第三方插件。聊到一半想去看看文件、拖个东西进去、或者在编辑器里打开某个文件时，不用再去翻路径手动 `cd`。

<details open>
<summary><b>中文</b></summary>

## 前置要求

- dsh `>= 0.1.1-rc.2`
- `pnpm` 可用（`dsh plugin` 底层转发给 pnpm）

## 安装

最省事的办法是用[插件市场](https://github.com/EasyTZ/dsh-market)：打开「发现」，搜 `explorer`，点「安装」。

命令行：

```sh
dsh plugin --profile <name> add @easytz/dsh-reveal-explorer
```

`<name>` 是**必填**的 profile 名，不能省略——桌面版通常是 `web`，TUI 是 `tui`；不确定就看 `$DSH_HOME/profiles/` 下的目录名。想钉死版本就写 `@easytz/dsh-reveal-explorer@0.2.3`。

插件自带 `dsh.bundle` 层（`cordis.patch.yml`），`dsh plugin add` 会同时完成「装进去」和「注册激活」，**不需要手写 patch**。

装完重启 dsh，会话头部的工具区出现「在资源管理器中打开」按钮。

## 用法

点会话头部工具区的**在资源管理器中打开**按钮，就用系统自带的文件管理器打开**当前会话**的工作区目录：

- Windows → 资源管理器（`explorer.exe`）
- macOS → 访达（`open`）
- Linux → 默认文件管理器（`xdg-open`）

打开的是当前会话绑定的那个工作区——切到别的会话再点，开的就是那个会话的目录。目录在注册之后被移动或删除时，按钮会明确提示目录不存在，而不是假装打开成功。

## 卸载

```sh
dsh plugin --profile <name> remove @easytz/dsh-reveal-explorer
```

`<name>` 与安装时一致。`remove` 会把包从 profile 依赖里移除，dsh 随后会把它从激活清单（`dsh.profile.bundles`）里撤掉。重启 dsh 后按钮消失。

> 如果你按旧版 README 手动往 `$DSH_HOME/profiles/<name>/cordis.patch.yml` 或 `$DSH_HOME/cordis.patch.yml` 里加过 `- insert:` 条目，卸载时把那段 YAML 一起删掉。

## 已知限制

- 工作区目录在注册之后被移动 / 删除时，插件会在调用前检测并提示目录不存在，而不是对着错误弹「已打开」。
- Windows 上 `explorer.exe` 有「窗口确实打开了却返回非零退出码」的怪癖，插件对此做了容错（只有进程起不来才算失败）。

## 平台支持

目前只在 Windows（`explorer.exe`）上验证过；macOS（`open`）与 Linux（`xdg-open`）的分支已写好但未实测，欢迎反馈。

</details>

<details>
<summary><b>English</b></summary>

A third-party plugin for [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (dsh): adds a button to the session header that opens the current workspace directory in your system file manager.

### Requirements

- dsh `>= 0.1.1-rc.2`
- `pnpm` available (`dsh plugin` shells out to pnpm)

### Install

Easiest path is the [plugin market](https://github.com/EasyTZ/dsh-market): open **Discover**, search `explorer`, hit **Install**.

From the command line:

```sh
dsh plugin --profile <name> add @easytz/dsh-reveal-explorer
```

`<name>` is **required** — your dsh profile (usually `web` for the desktop/web UI, `tui` for the TUI). The package ships its own `dsh.bundle` layer, so `dsh plugin add` both installs **and** activates it.

Restart dsh — an "open in file manager" button appears in the session header toolbar.

### Usage

Click the button to open the **current session's** workspace directory: Explorer on Windows, Finder on macOS (`open`), your default file manager on Linux (`xdg-open`). Switch sessions and the button follows — it always opens the directory bound to the session you're looking at. If that directory has been moved or deleted, you get a clear "directory does not exist" message rather than a fake success.

### Uninstall

```sh
dsh plugin --profile <name> remove @easytz/dsh-reveal-explorer
```

### Limitations

- If the workspace directory is moved or deleted after registration, the plugin detects it up front and says so.
- On Windows `explorer.exe` can return a non-zero exit code even when the window did open; the plugin tolerates that (only a failure to spawn counts as an error).
- Verified on Windows only; the macOS and Linux paths are written but untested. Feedback welcome.

</details>

## 许可证 / License

[MIT](LICENSE)
