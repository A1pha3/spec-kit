# Spec Kit 快速上手指南

本文档将引导你快速开始使用 Spec Kit，体验规范驱动开发的强大能力。

## 🎯 六步开发流程

Spec Kit 采用完整的六步开发流程：

1. **安装 Specify CLI** - 初始化项目环境
2. **建立项目原则** - 创建项目治理原则
3. **创建规范** - 定义需求和目标
4. **制定计划** - 规划技术实现方案
5. **分解任务** - 将计划转化为具体任务
6. **执行实施** - 构建功能

## 📋 环境要求

### 基础环境
- **操作系统**: Linux/macOS/Windows
- **Python**: 3.11+ (推荐使用 [uv](https://docs.astral.sh/uv/) 进行包管理)
- **Git**: 版本控制系统

### AI 助手选择（至少一个）
- [Claude Code](https://www.anthropic.com/claude-code)
- [GitHub Copilot](https://code.visualstudio.com/)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [Cursor](https://cursor.sh/)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [opencode](https://opencode.ai/)
- [Windsurf](https://windsurf.com/)
- [Kilo Code](https://github.com/Kilo-Org/kilocode)
- [Auggie CLI](https://docs.augmentcode.com/cli/overview)
- [CodeBuddy CLI](https://www.codebuddy.ai/cli)
- [Roo Code](https://roocode.com/)
- [Codex CLI](https://github.com/openai/codex)
- [Amazon Q Developer CLI](https://aws.amazon.com/developer/learning/q-developer-cli/)

## 🚀 第一步：安装 Specify CLI

### 方法一：持久化安装（推荐）

安装一次，随处使用：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

然后直接使用工具：

```bash
specify init <项目名称>
specify check
```

### 方法二：一次性使用

无需安装直接运行：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <项目名称>
```

### 初始化新项目

```bash
# 基本项目初始化
specify init my-project

# 在当前目录初始化
specify init .
# 或使用 --here 参数
specify init --here
```

### 指定 AI 助手

```bash
# 指定 Claude Code
specify init my-project --ai claude

# 指定 GitHub Copilot
specify init my-project --ai copilot

# 指定 Cursor
specify init my-project --ai cursor-agent

# 指定 Windsurf
specify init my-project --ai windsurf

# 指定 Amp
specify init my-project --ai amp
```

### 脚本类型选择

```bash
# 强制使用 Bash 脚本
specify init my-project --script sh

# 强制使用 PowerShell 脚本
specify init my-project --script ps
```

## 📝 第二步：建立项目原则

启动你的 AI 助手。使用 `/speckit.constitution` 命令创建项目的治理原则：

```bash
/speckit.constitution 创建专注于代码质量、测试标准、用户体验一致性和性能要求的原则
```

## 📋 第三步：创建规范

使用 `/speckit.specify` 命令描述你想要构建的内容，专注于**做什么**和**为什么**，而不是技术细节。

### 规范示例：照片管理应用

```bash
/speckit.specify 构建一个帮助我整理照片到相册的应用。相册按日期分组，可以在主页上通过拖放重新组织。相册不会嵌套在其他相册中。在每个相册内，照片以平铺界面预览。
```

### 规范示例：团队任务管理平台

```bash
/speckit.specify 开发 Taskify，一个团队生产力平台。它应该允许用户创建项目、添加团队成员、分配任务、评论并在看板风格的任务板之间移动任务。在这个初始阶段，我们将有多个用户，但用户会预先声明，预定义。我想要五个用户分为两个类别，一个产品经理和四个工程师。让我们创建三个不同的示例项目。让我们为每个任务的状态设置标准的看板列，如"待办"、"进行中"、"评审中"和"已完成"。此应用不需要登录，因为这只是确保基本功能设置的最初测试。对于任务卡片，你应该能够更改任务的当前状态到不同的看板列。你应该能够为特定卡片留下无限数量的评论。你应该能够从该任务卡片分配一个有效用户。当你首次启动 Taskify 时，它会显示五个用户的列表供选择。不需要密码。当你点击用户时，进入主视图，显示项目列表。当你点击项目时，打开该项目的看板。你将看到列。你将能够在不同列之间拖放卡片。你将看到分配给当前登录用户的所有卡片以不同颜色显示，以便快速识别你的任务。你可以编辑你发表的任何评论，但不能编辑其他人的评论。你可以删除你发表的任何评论，但不能删除其他人的评论。
```

## 🏗️ 第四步：制定技术实现计划

使用 `/speckit.plan` 命令提供技术栈和架构选择。

### 计划示例

```bash
/speckit.plan 应用使用 Vite，尽可能使用最少的库。尽可能使用原生 HTML、CSS 和 JavaScript。图片不上传到任何地方，元数据存储在本地 SQLite 数据库中。
```

### 技术栈示例

```bash
/speckit.plan 我们将使用 .NET Aspire 生成此应用，使用 Postgres 作为数据库。前端应使用 Blazor 服务器，具有拖放任务板和实时更新。应创建包含项目 API、任务 API 和通知 API 的 REST API。
```

## 🔧 第五步：分解任务

使用 `/speckit.tasks` 从实施计划创建可操作的任务列表。

```bash
/speckit.tasks
```

## 🚀 第六步：执行实施

使用 `/speckit.implement` 执行所有任务，根据计划构建功能。

```bash
/speckit.implement
```

## 🎯 完整示例：构建 Taskify

### 步骤 1：定义需求

使用 `/speckit.specify` 详细描述 Taskify 的功能需求。

### 步骤 2：完善规范

澄清任何缺失的需求：

```bash
/speckit.specify 对于每个示例项目或创建的项目，每个项目应有 5 到 15 个任务，随机分布到不同的完成状态。确保每个完成阶段至少有一个任务。
```

验证规范检查清单：

```bash
/speckit.specify 阅读审查和验收检查清单，如果功能规范符合标准，请勾选每个项目。如果不符合，请留空。
```

### 步骤 3：生成技术计划

使用 `/speckit.plan` 指定技术细节。

### 步骤 4：验证和实施

让 AI 助手审核实施计划：

```bash
/speckit.tasks 现在我希望你审核实施计划和实施细节文件。通读它，重点关注确定是否需要执行一系列任务。因为我不确定这里是否有足够的信息。
```

最后实施解决方案：

```bash
/speckit.implement
```

## 🔧 可用斜杠命令

运行 `specify init` 后，你的 AI 编码助手将可以使用以下斜杠命令：

### 核心命令
- `/speckit.constitution` - 创建或更新项目治理原则
- `/speckit.specify` - 定义要构建的内容
- `/speckit.plan` - 创建技术实施计划
- `/speckit.tasks` - 生成可操作任务列表
- `/speckit.implement` - 执行所有任务构建功能

### 可选命令
- `/speckit.clarify` - 澄清未明确指定的区域
- `/speckit.analyze` - 跨工件一致性和覆盖分析
- `/speckit.checklist` - 生成自定义质量检查清单

## 🔑 关键原则

### 1. 明确性
- 明确说明你要构建的内容和原因
- 在规范阶段不要关注技术栈

### 2. 迭代和优化
- 在实施前迭代和优化你的规范
- 验证计划后再开始编码

### 3. 信任 AI 助手
- 让 AI 助手处理实施细节
- 专注于高层次的需求定义

## 🚨 常见错误与解决方案

### 错误 1：网络连接问题

**症状**: 初始化时出现网络错误
**解决方案**: 
```bash
# 使用离线模式
specify init my-project --offline

# 或检查网络连接
curl -I https://api.github.com
```

### 错误 2：AI 助手工具未检测到

**症状**: 工具检测失败
**解决方案**:
```bash
# 忽略工具检测
specify init my-project --ai claude --ignore-agent-tools

# 或手动安装所需工具
```

### 错误 3：脚本执行权限问题（Linux/macOS）

**症状**: 脚本无法执行
**解决方案**:
```bash
# 确保脚本有执行权限
chmod +x .specify/scripts/*.sh
```

### 错误 4：Git 认证问题（Linux）

**症状**: Git 操作失败
**解决方案**:
```bash
# 安装 Git Credential Manager
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
git config --global credential.helper manager
```

## ✅ 验证安装成功

初始化完成后，你应该在 AI 助手中看到以下命令可用：

- `/speckit.constitution` - 创建项目原则
- `/speckit.specify` - 创建规范
- `/speckit.plan` - 生成实施计划  
- `/speckit.tasks` - 分解为可操作任务
- `/speckit.implement` - 执行实施

`.specify/scripts` 目录将包含 `.sh` 和 `.ps1` 脚本。

## 🎯 下一步行动

完成快速入门后，建议：

1. **深入学习 CLI**: 阅读 [CLI 命令详解](./cli-reference.md) 掌握所有功能
2. **理解架构**: 查看 [代码结构解析](./code-structure.md) 了解内部实现
3. **掌握理念**: 学习 [设计哲学与最佳实践](./design-philosophy.md) 深入理解规范驱动开发
4. **解决疑问**: 查阅 [常见问题解答](./faq.md) 解决使用中的问题

## 💡 小贴士

- 从简单项目开始，逐步增加复杂度
- 充分利用 AI 助手的交互式指导
- 定期验证规范与实现的一致性
- 参与社区讨论分享经验

---

*准备好开始你的规范驱动开发之旅了吗？从创建第一个规范开始吧！*