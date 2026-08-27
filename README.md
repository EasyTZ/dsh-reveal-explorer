# dsh-reveal-explorer

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（下称 dsh）的第三方插件：在**会话头部**加一个按钮，点击后用系统文件管理器打开当前工作区目录。

## 前置要求

- dsh `>= 0.1.1-rc.2`（peer 依赖：`@deepseek-ai/cordis ^4.0.1`、`@deepseek-ai/dsh-host-webserver ^0.1.1-rc.2`、`@deepseek-ai/dsh-workspace ^0.1.1-rc.2`）

## 安装

「装进去」和「打开它」是两件事，缺一不可：

```sh
dsh plugin --profile <name> add github:EasyTZ/dsh-reveal-explorer#v0.1.1
```

> **必须写 GitHub 地址，不能只写包名。** `dsh plugin add` 会把参数原样转给 pnpm，只写 `dsh-reveal-explorer` 会去 npm registry 找同名包 —— 那可能是别人的包（`dsh-git` 在 npm 上就已被他人占用）。换个 tag 就是换版本；想跟最新可以用 `#main`，但**不建议**：钉 tag 才能复现。

## 激活

往 patch 层文件（`$DSH_HOME/profiles/<name>/cordis.patch.yml` 或机器级 `$DSH_HOME/cordis.patch.yml`）里加一条 `- insert:` 条目：

```yaml
- insert:
    - id: dsh-reveal-explorer
      name: 'dsh-reveal-explorer'
```

> **`id` 别用通用词**（比如 `reveal-explorer`）。`- insert:` 不去重：一旦与 dsh 自带 bundle 里某条条目同名，cordis loader 会抛 `duplicate loader entry id`，**内核直接退出**。dsh 自带的 id 里有大量 `git` / `session` / `settings` / `storage` 这类通用词，而且内核会自行更新到新版本 —— 撞车只是时间问题。直接拿包名当 id 最省事。

重启 dsh 后，会话头部的工具区会出现「在资源管理器中打开」按钮。

## 已知限制

- 工作区目录在注册之后被移动 / 删除时，插件会在调用前检测并提示目录不存在，而不是对着错误弹「已打开」。
- Windows 上 `explorer.exe` 有「窗口确实打开了却返回非零退出码」的怪癖，插件对此做了容错（只有进程起不来才算失败）。

## 平台支持

目前只在 Windows（`explorer.exe`）上验证过；macOS（`open`）与 Linux（`xdg-open`）的分支已写好但未实测，欢迎反馈。
