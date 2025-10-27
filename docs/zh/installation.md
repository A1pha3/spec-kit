# 安装与配置指南

本文档详细介绍了 Spec Kit 的安装和配置过程，帮助您快速搭建开发环境。

## 📋 环境要求

### 基础环境
- **操作系统**: Linux/macOS/Windows
- **Python**: 3.11+ (推荐使用 [uv](https://docs.astral.sh/uv/) 进行包管理)
- **Git**: 版本控制系统

### AI 助手选择（至少一个）
- [Claude Code](https://www.anthropic.com/claude-code)
- [GitHub Copilot](https://code.visualstudio.com/)
- [Codebuddy CLI](https://www.codebuddy.ai/cli)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)

## 🚀 安装步骤

### 方法一：使用 uvx 直接运行（推荐）

这是最简单的方式，无需安装即可使用：

```bash
# 初始化新项目
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称>

# 在当前目录初始化
uvx --from git+https://github.com/github/spec-kit.git specify init .
# 或使用 --here 参数
uvx --from git+https://github.com/github/spec-kit.git specify init --here
```

### 方法二：持久化安装

安装后可在任何地方使用：

```bash
# 安装 Specify CLI
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 使用安装的命令
specify init <项目名称>

# 升级到最新版本
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

## ⚙️ 配置选项

### 指定 AI 助手

```bash
# 指定 Claude Code
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --ai claude

# 指定 GitHub Copilot
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --ai copilot

# 指定 Gemini
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --ai gemini

# 指定 Codebuddy
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --ai codebuddy
```

### 脚本类型选择

```bash
# 强制使用 Bash 脚本
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --script sh

# 强制使用 PowerShell 脚本
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --script ps
```

### 其他选项

```bash
# 忽略 AI 助手工具检测
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --ai claude --ignore-agent-tools

# 跳过 Git 初始化
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --no-git

# 强制覆盖当前目录
uvx --from git+https://github.com/github/spec-kit.git specify init . --force

# 启用调试输出
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --debug
```

## 🔧 环境验证

### 检查工具安装

```bash
# 检查所有必需工具
specify check
```

输出示例：
```
🔍 工具检查报告

✅ Git: 已安装 (版本 2.39.2)
✅ Claude Code: 已安装 (版本 1.0.0)
⚠️  VS Code: 未检测到（可选）

所有必需工具已就绪！
```

### 验证安装成功

初始化完成后，检查以下内容：

1. **项目结构**：
```bash
ls -la
# 应该看到 .specify/ 目录和项目文件
```

2. **AI 助手命令**：
启动你的 AI 助手，检查是否出现以下命令：
- `/speckit.specify` - 创建规范
- `/speckit.plan` - 生成计划
- `/speckit.tasks` - 分解任务

3. **脚本权限**（Linux/macOS）：
```bash
ls -la .specify/scripts/
# 确保 .sh 文件有执行权限
```

## 🚨 常见安装问题

### 问题 1: 网络连接错误

**症状**: 初始化时出现网络超时或连接错误

**解决方案**:
```bash
# 使用离线模式
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称> --offline

# 或配置代理
set HTTPS_PROXY=http://your-proxy:port  # Windows
export HTTPS_PROXY=http://your-proxy:port  # Linux/macOS
```

### 问题 2: Git 认证问题（Linux）

**症状**: Git 操作失败

**解决方案**:
```bash
# 安装 Git Credential Manager
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
git config --global credential.helper manager
```

### 问题 3: 权限错误

**症状**: 文件操作权限不足

**解决方案**:
```bash
# 检查目录权限
ls -la

# 修复权限（Linux/macOS）
chmod 755 .
chmod +x .specify/scripts/*.sh
```

### 问题 4: Python 版本不兼容

**症状**: Python 版本低于 3.11

**解决方案**:
```bash
# 检查 Python 版本
python --version

# 安装 Python 3.11+
# Ubuntu/Debian
sudo apt update && sudo apt install python3.11

# macOS
brew install python@3.11

# Windows
# 从 Python 官网下载安装包
```

## 🔄 更新和维护

### 更新 Spec Kit

```bash
# 如果使用 uvx
# 无需更新，每次都会使用最新版本

# 如果使用持久化安装
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

### 清理缓存

```bash
# 清理模板缓存
rm -rf ~/.cache/spec-kit/

# 清理项目文件（如果需要重新开始）
rm -rf .specify/ specs/
```

## 🎯 最佳实践

### 1. 选择合适的安装方式
- **快速体验**: 使用 `uvx` 直接运行
- **日常使用**: 使用持久化安装
- **开发环境**: 从源码安装

### 2. 环境配置建议
- **统一 AI 助手**: 团队项目使用相同的 AI 助手
- **脚本类型**: 根据操作系统选择合适的脚本类型
- **版本控制**: 使用 Git 进行版本管理

### 3. 故障排除
- **启用调试**: 使用 `--debug` 参数获取详细信息
- **检查日志**: 查看错误日志定位问题
- **社区支持**: 在 GitHub Issues 寻求帮助

## 📚 下一步

安装完成后，建议：

1. **快速开始**: 阅读 [快速上手指南](./quickstart.md) 创建第一个项目
2. **深入学习**: 查看 [CLI 命令详解](./cli-reference.md) 掌握所有功能
3. **理解理念**: 学习 [设计哲学与最佳实践](./design-philosophy.md) 深入理解规范驱动开发

---

*现在你已经成功安装了 Spec Kit，开始你的规范驱动开发之旅吧！*