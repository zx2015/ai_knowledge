---
title: GitHub CLI (gh) 代理配置指南
created_at: 2026-04-11
updated_at: 2026-04-11
tags: [GitHub, CLI, Proxy, Network]
---

# GitHub CLI (gh) 代理配置

GitHub CLI (`gh`) 本身**没有**类似 `gh config set proxy` 的直接命令来配置代理。它主要通过**系统环境变量**或底层的 **Git 配置**来实现代理功能。

## 1. 使用环境变量（推荐）
这是 GitHub CLI 官方支持的标准方式。`gh` 会自动识别系统标准的代理环境变量。

### Linux / macOS (Bash/Zsh)
在终端中执行以下命令（或写入 `~/.bashrc` 或 `~/.zshrc`）：
```bash
export HTTPS_PROXY=http://127.0.0.1:7890
export HTTP_PROXY=http://127.0.0.1:7890
# 可选：配置不使用代理的地址
export NO_PROXY=localhost,127.0.0.1,github.com
```

### Windows (PowerShell)
```powershell
$env:HTTPS_PROXY="http://127.0.0.1:7890"
$env:HTTP_PROXY="http://127.0.0.1:7890"
```

## 2. 配置 Git 代理
由于 `gh` 在执行 `gh repo clone` 或 `gh pr checkout` 等涉及代码同步的操作时会调用底层的 `git` 命令，因此配置 Git 代理对这些操作同样有效。

- **设置全局 Git 代理**：
  ```bash
  git config --global http.proxy http://127.0.0.1:7890
  git config --global https.proxy http://127.0.0.1:7890
  ```
- **查看配置**：`git config --global --get http.proxy`
- **取消设置**：`git config --global --unset http.proxy`

## 3. SSH 协议代理
如果你配置 `gh` 使用 SSH 协议（`gh config set git_protocol ssh`），则需要在 `~/.ssh/config` 中配置代理：

```text
Host github.com
    Hostname github.com
    User git
    # Linux/macOS 下使用 nc (netcat)
    ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p
```

## 4. 验证配置
执行以下命令检查连接状态：
```bash
gh auth status
# 或
gh repo list --limit 1
```

## 注意事项
- **SOCKS5 代理**：如果使用 SOCKS5，格式为 `socks5://127.0.0.1:7890`。
- **用户名密码**：如果代理需要认证，格式为 `http://username:password@proxy.host:port`。

## 参考链接
- [GitHub CLI Manual - gh config](https://cli.github.com/manual/gh_config)
- [GitHub CLI Environment Variables](https://cli.github.com/manual/gh_help_environment)

## Update History
- 2026-04-11: 初次创建，明确 gh 官方不支持通过 gh config 直接设置 proxy 键，需使用环境变量。
