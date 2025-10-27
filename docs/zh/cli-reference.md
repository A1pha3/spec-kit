# CLI 命令详解

本文档详细介绍了 Spec Kit 提供的所有命令行工具及其用法，基于实际的 `specify` CLI 实现。

## 🎯 命令概览

Spec Kit 提供以下主要命令：

| 命令 | 功能 | 常用参数 |
|------|------|----------|
| `specify init` | 初始化新项目 | `--ai`, `--script`, `--here`, `--offline` |
| `specify check` | 检查所需工具 | 无参数 |

## 🔧 详细命令说明

### `specify init` - 项目初始化

初始化一个新的 Spec Kit 项目，配置 AI 助手和开发环境。

**语法**:
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init [PROJECT_NAME] [OPTIONS]
```

**参数**:
- `PROJECT_NAME`: 项目名称（可选，默认为当前目录）

**选项**:
- `--ai TEXT`: 指定 AI 助手 (`claude`, `copilot`, `gemini`, `codebuddy`)
- `--script TEXT`: 脚本类型 (`sh`, `ps`)
- `--here`: 在当前目录初始化
- `--offline`: 离线模式，使用本地缓存
- `--ignore-agent-tools`: 忽略 AI 助手工具检测
- `--github-token TEXT`: GitHub 访问令牌

**示例**:
```bash
# 基本初始化（交互式选择 AI 助手）
uvx --from git+https://github.com/github/spec-kit.git specify init my-project

# 指定 Claude Code 助手
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --ai claude

# 在当前目录初始化
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai copilot

# 强制使用 PowerShell 脚本
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --script ps

# 离线初始化
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --offline
```

**交互流程**:
1. **项目检测**: 检查当前目录状态
2. **AI 助手选择**: 交互式选择或使用 `--ai` 参数指定
3. **脚本类型选择**: 自动检测或使用 `--script` 参数指定
4. **工具检测**: 检查所需工具是否安装（可跳过）
5. **模板下载**: 从 GitHub 下载对应模板
6. **项目创建**: 解压模板并配置项目

### `specify check` - 工具检查

检查开发环境所需的工具是否已安装。

**语法**:
```bash
uvx --from git+https://github.com/github/spec-kit.git specify check
```

**检查内容**:
- **Git**: 版本控制系统
- **AI 助手 CLI 工具**: 根据配置的 AI 助手检查相应工具
- **VS Code**: 代码编辑器（可选）

**输出示例**:
```
🔍 工具检查报告

✅ Git: 已安装 (版本 2.39.2)
✅ Claude Code: 已安装 (版本 1.0.0)
⚠️  VS Code: 未检测到（可选）

所有必需工具已就绪！
```

## 🎛️ 命令参数详解

### AI 助手选项 (`--ai`)

Spec Kit 支持多种 AI 助手，每个助手对应不同的模板和配置：

| 助手 | 描述 | 所需工具 |
|------|------|----------|
| `claude` | Claude Code | Claude Code CLI |
| `copilot` | GitHub Copilot | VS Code + Copilot 扩展 |
| `gemini` | Google Gemini | Gemini CLI |
| `codebuddy` | Codebuddy | Codebuddy CLI |

**使用示例**:
```bash
# 使用 Claude Code
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --ai claude

# 使用 GitHub Copilot
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --ai copilot
```

### 脚本类型选项 (`--script`)

Spec Kit 支持两种脚本类型，自动根据操作系统选择：

| 脚本类型 | 描述 | 默认平台 |
|----------|------|----------|
| `sh` | POSIX Shell (Bash/Zsh) | Linux/macOS |
| `ps` | PowerShell | Windows |

**使用示例**:
```bash
# 强制使用 Bash 脚本
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --script sh

# 强制使用 PowerShell 脚本
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --script ps
```

### 初始化位置选项 (`--here`)

在当前目录初始化项目，而不是创建新目录。

**使用场景**:
- 在现有目录中初始化 Spec Kit
- 将现有项目转换为 Spec Kit 项目
- 避免创建额外的目录层级

**使用示例**:
```bash
# 进入目标目录
cd existing-project

# 在当前目录初始化
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai claude
```

### 离线模式 (`--offline`)

使用本地缓存的模板进行初始化，无需网络连接。

**使用场景**:
- 网络连接不稳定
- 在隔离环境中使用
- 快速重复初始化

**使用示例**:
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --offline
```

## 🏗️ 初始化后的项目结构

初始化完成后，项目将包含以下结构：

```
my-project/
├── .specify/               # Spec Kit 配置目录
│   └── scripts/           # 自动化脚本
│       ├── *.sh          # Bash 脚本
│       └── *.ps1         # PowerShell 脚本
├── specs/                 # 规范定义目录
│   ├── plan.md           # 项目计划模板
│   └── spec.md           # 规范模板
├── .vscode/              # VS Code 配置
│   └── settings.json     # 编辑器设置
└── README.md             # 项目说明
```

### AI 助手命令

初始化后，你的 AI 助手中将添加以下命令：

- `/speckit.specify` - 创建规范定义
- `/speckit.plan` - 生成技术实施计划
- `/speckit.tasks` - 分解为可操作任务

## 🔧 高级配置

### GitHub Token 配置

如果需要访问私有仓库或避免 API 限制，可以配置 GitHub Token：

**方法 1: 环境变量**
```bash
export GH_TOKEN=your_github_token
# 或
export GITHUB_TOKEN=your_github_token
```

**方法 2: 命令行参数**
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --github-token your_github_token
```

### 忽略工具检测

如果不想检查 AI 助手工具，可以使用 `--ignore-agent-tools`：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --ai claude --ignore-agent-tools
```

## 🚨 常见问题与解决方案

### 问题 1: 网络连接错误

**症状**: 初始化时出现网络超时或连接错误
**解决方案**:
```bash
# 使用离线模式
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --offline

# 或配置代理
set HTTPS_PROXY=http://your-proxy:port  # Windows
export HTTPS_PROXY=http://your-proxy:port  # Linux/macOS
```

### 问题 2: GitHub API 限制

**症状**: 收到 GitHub API 速率限制错误
**解决方案**:
```bash
# 配置 GitHub Token
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --github-token your_token

# 或使用离线模式
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --offline
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

### 问题 4: 模板下载失败

**症状**: 模板 ZIP 文件下载或解压失败
**解决方案**:
```bash
# 清理缓存并重试
rm -rf ~/.cache/spec-kit/
uvx --from git+https://github.com/github/spec-kit.git specify init my-project
```

## 📊 命令输出示例

### 成功初始化输出

```
🚀 正在初始化 Spec Kit 项目...

📋 项目信息
  名称: my-project
  路径: /path/to/my-project

🤖 AI 助手配置
  选择: Claude Code
  状态: ✅ 已安装

📜 脚本类型
  检测: POSIX Shell (Bash)
  状态: ✅ 支持

🔧 工具检测
  Git: ✅ 已安装
  Claude Code: ✅ 已安装
  VS Code: ⚠️ 未检测到（可选）

📥 下载模板
  来源: GitHub Release
  状态: ✅ 下载完成

🏗️ 创建项目
  状态: ✅ 项目创建成功

🎉 初始化完成！

接下来步骤：
1. 使用 /speckit.specify 创建规范
2. 使用 /speckit.plan 制定技术计划
3. 使用 /speckit.tasks 分解实施任务

快乐编码！ 🎯
```

## 🔄 环境变量

Spec Kit 支持以下环境变量：

| 变量名 | 描述 | 默认值 |
|--------|------|--------|
| `GH_TOKEN` | GitHub 访问令牌 | 无 |
| `GITHUB_TOKEN` | GitHub 访问令牌 | 无 |
| `SPEC_KIT_OFFLINE` | 强制离线模式 | `false` |

## 🎯 最佳实践

### 1. 选择合适的 AI 助手
- **Claude Code**: 功能强大，CLI 工具完善
- **GitHub Copilot**: VS Code 集成度高
- **Gemini**: Google 生态系统集成
- **Codebuddy**: 专用 AI 开发工具

### 2. 脚本类型选择
- **Linux/macOS**: 默认使用 Bash 脚本
- **Windows**: 默认使用 PowerShell 脚本
- **跨平台项目**: 建议同时提供两种脚本

### 3. 项目初始化策略
- **新项目**: 使用 `specify init <project-name>`
- **现有项目**: 使用 `specify init --here`
- **团队项目**: 统一 AI 助手和脚本类型配置

### 4. 网络优化
- **稳定网络**: 直接下载最新模板
- **不稳定网络**: 使用 `--offline` 模式
- **企业环境**: 配置内部镜像或缓存

## 🔍 调试技巧

### 启用详细输出

```bash
# 设置详细模式（如果支持）
uvx --from git+https://github.com/github/spec-kit.git specify init my-project --verbose
```

### 检查工具状态

```bash
# 单独检查工具
uvx --from git+https://github.com/github/spec-kit.git specify check
```

### 查看帮助信息

```bash
# 查看命令帮助
uvx --from git+https://github.com/github/spec-kit.git specify --help
uvx --from git+https://github.com/github/spec-kit.git specify init --help
```

## 📚 下一步学习

完成 CLI 命令学习后，建议：

1. **深入理解架构**: 阅读 [代码结构解析](./code-structure.md) 了解内部实现
2. **掌握开发理念**: 学习 [设计哲学与最佳实践](./design-philosophy.md)
3. **解决实际问题**: 查阅 [常见问题解答](./faq.md)
4. **参与社区**: 加入讨论分享使用经验

---

*现在你已经掌握了 Spec Kit CLI 的所有功能，开始创建你的第一个规范驱动项目吧！*