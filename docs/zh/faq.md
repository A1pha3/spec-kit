# 常见问题解答

本文档收集了 Spec Kit 使用过程中的常见问题及其解决方案。

## 安装与配置

### Q: 如何安装 Spec Kit？

**A**: 有多种安装方式：

1. **从源码安装**（推荐用于开发）：
```bash
git clone https://github.com/spec-kit/spec-kit.git
cd spec-kit
pip install -e .
```

2. **使用 pip 安装**（如果已发布到 PyPI）：
```bash
pip install spec-kit
```

3. **使用 uv 安装**：
```bash
uv tool install --from specify-cli.py specify-cli
```

### Q: 安装后无法运行 `specify` 命令怎么办？

**A**: 检查以下可能原因：

1. **Python 环境问题**：
```bash
python --version  # 确保 Python 3.8+
which specify     # 检查命令路径
```

2. **PATH 配置问题**：
```bash
# 检查 Python 脚本目录是否在 PATH 中
echo $PATH | grep -i "python"

# 手动添加（Linux/macOS）
export PATH="$HOME/.local/bin:$PATH"
```

3. **重新安装**：
```bash
pip uninstall spec-kit
pip install spec-kit
```

### Q: 如何验证安装是否成功？

**A**: 运行版本检查命令：
```bash
specify --version
```

如果看到版本号输出，说明安装成功。

## 项目初始化

### Q: `specify init` 命令失败，提示网络错误

**A**: 网络连接问题的解决方案：

1. **使用离线模式**：
```bash
specify init --offline
```

2. **检查网络连接**：
```bash
# 测试 GitHub 连接
curl -I https://api.github.com

# 如果使用代理
export HTTPS_PROXY=http://your-proxy:port
```

3. **手动下载模板**：
```bash
# 查看可用模板
specify template list

# 手动下载
specify template download template-name
```

### Q: 初始化时提示 "Directory already exists"

**A**: 目录冲突的解决方案：

1. **使用不同名称**：
```bash
specify init new-project-name
```

2. **在当前目录初始化**：
```bash
# 进入目标目录
cd existing-directory

# 在当前目录初始化
specify init --here
```

3. **强制覆盖**（谨慎使用）：
```bash
specify init --here --force
```

### Q: 如何选择适合的 AI 助手？

**A**: 根据你的开发环境选择：

- **GitHub Copilot**：VS Code 用户，集成度高
- **Claude Code**：需要 CLI 工具，功能强大
- **Cursor**：专用 AI IDE，体验优秀
- **其他助手**：根据具体需求选择

**检查工具可用性**：
```bash
specify check
```

## 规范管理

### Q: 规范文件应该放在哪里？

**A**: 推荐的标准目录结构：

```
project/
├── specs/           # 主要规范目录
│   ├── product/    # 产品需求规范
│   ├── technical/  # 技术规范
│   └── api/        # API 规范
├── src/            # 源代码
└── tests/          # 测试文件
```

### Q: 规范文件应该使用什么格式？

**A**: 推荐使用 Markdown 格式，包含以下结构：

```markdown
# 功能名称

## 用户故事 1: 标题

**优先级**: P1
**描述**: 用户视角描述

### 验收标准
- [ ] 具体验收条件
- [ ] 可验证的行为

### 技术规范
- **API 端点**: 方法路径
- **数据模型**: 结构描述
- **验证规则**: 输入要求
```

### Q: 如何验证规范的质量？

**A**: 使用以下方法：

1. **语法检查**：
```bash
specify validate specs/
```

2. **完整性检查**：
```bash
specify status --verbose
```

3. **一致性检查**：
```bash
specify validate --strict
```

## CLI 命令问题

### Q: `specify status` 显示不准确

**A**: 状态显示问题的排查：

1. **检查规范文件路径**：
```bash
# 显示详细路径
specify status --verbose
```

2. **验证规范语法**：
```bash
specify validate --strict
```

3. **更新状态缓存**：
```bash
# 删除状态缓存文件
rm -rf .speckit/cache/
```

### Q: `specify generate-tasks` 生成的任务不完整

**A**: 任务生成问题的解决：

1. **检查规范完整性**：
```bash
# 验证规范语法
specify validate spec-file.md
```

2. **使用详细模式**：
```bash
specify generate-tasks spec-file.md --verbose
```

3. **检查模板配置**：
```bash
# 查看当前模板
specify template info current-template
```

### Q: 命令执行缓慢

**A**: 性能优化建议：

1. **启用缓存**：
```bash
# 检查缓存配置
cat .speckit/config.yaml
```

2. **减少网络请求**：
```bash
# 使用离线模式
specify command --offline
```

3. **优化规范文件**：
```bash
# 分割大规范文件
# 避免嵌套过深的目录结构
```

## 模板相关问题

### Q: 如何创建自定义模板？

**A**: 自定义模板创建步骤：

1. **创建模板结构**：
```bash
mkdir my-template
cd my-template

# 创建标准目录结构
mkdir -p .specify/scripts specs src tests
```

2. **添加模板文件**：
```bash
# 创建模板配置文件
echo 'name: "My Custom Template"' > .speckit/template.yaml

# 添加示例规范
echo '# 示例规范' > specs/example.md
```

3. **打包模板**：
```bash
zip -r my-template.zip .
```

4. **使用自定义模板**：
```bash
specify init --template ./my-template.zip
```

### Q: 模板下载失败

**A**: 模板下载问题的解决：

1. **检查模板名称**：
```bash
specify template list
```

2. **手动下载**：
```bash
# 查看模板信息
specify template info template-name

# 手动下载
specify template download template-name
```

3. **使用本地模板**：
```bash
specify init --template /path/to/template.zip
```

## 集成与扩展

### Q: 如何将 Spec Kit 集成到 CI/CD？

**A**: CI/CD 集成示例：

**GitHub Actions**：
```yaml
name: Spec Validation
on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install Spec Kit
        run: pip install spec-kit
      - name: Validate Specs
        run: specify validate --strict
```

### Q: 如何扩展 Spec Kit 功能？

**A**: 功能扩展方式：

1. **自定义命令**：
```python
# 在 .speckit/plugins/ 目录创建插件
@app.command()
def my_command():
    """自定义命令"""
    console.print("Hello from custom command!")
```

2. **模板扩展**：
```bash
# 创建包含自定义脚本的模板
mkdir -p .specify/scripts/custom
```

3. **配置扩展**：
```yaml
# .speckit/config.yaml
custom:
  enabled: true
  settings:
    key: value
```

## 故障排除

### Q: 遇到 "Permission denied" 错误

**A**: 权限问题的解决：

1. **文件权限**：
```bash
# 检查文件权限
ls -la problematic-file

# 修复权限
chmod +x script-file.sh
```

2. **目录权限**：
```bash
# 检查目录权限
ls -la directory/

# 修复目录权限
chmod 755 directory/
```

3. **用户权限**：
```bash
# 检查当前用户
whoami

# 使用 sudo（谨慎）
sudo specify command
```

### Q: 内存不足错误

**A**: 内存优化建议：

1. **减少并发**：
```bash
# 使用单线程模式
specify command --single-thread
```

2. **优化规范文件**：
```bash
# 分割大文件
# 删除不必要的注释和空行
```

3. **增加系统内存**：
```bash
# 检查可用内存
free -h

# 增加交换空间
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
sudo mkswap /swapfile
sudo swapon /swapfile
```

### Q: 版本兼容性问题

**A**: 版本冲突的解决：

1. **检查版本**：
```bash
specify --version
python --version
```

2. **更新依赖**：
```bash
pip install --upgrade spec-kit
```

3. **使用虚拟环境**：
```bash
# 创建虚拟环境
python -m venv venv
source venv/bin/activate

# 安装 Spec Kit
pip install spec-kit
```

## 性能优化

### Q: 如何提高 Spec Kit 的性能？

**A**: 性能优化技巧：

1. **启用缓存**：
```yaml
# .speckit/config.yaml
cache:
  enabled: true
  ttl: 3600  # 缓存时间（秒）
```

2. **优化规范文件**：
- 避免过大的规范文件
- 使用引用而非复制
- 定期清理旧版本

3. **网络优化**：
```bash
# 使用本地镜像
specify init --mirror https://local-mirror.com
```

## 获取帮助

### Q: 在哪里可以找到更多帮助？

**A**: 帮助资源：

1. **官方文档**：
```bash
# 查看帮助
specify --help
specify init --help
```

2. **社区支持**：
- GitHub Issues：报告问题和建议
- 讨论区：与其他用户交流
- 文档：详细的使用指南

3. **调试信息**：
```bash
# 启用调试模式
specify command --debug

# 查看日志
cat ~/.specify/logs/specify.log
```

如果以上解决方案无法解决你的问题，请提供以下信息寻求帮助：

1. **错误信息**：完整的错误输出
2. **环境信息**：`specify --version` 和 `python --version`
3. **复现步骤**：导致问题的具体操作步骤
4. **相关文件**：涉及的规范文件内容

这样可以帮助我们更快地定位和解决问题。