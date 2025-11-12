# Git Hooks 版本控制管理

本项目提供了一套可版本控制的 Git Hooks 解决方案，帮助团队共享和管理 Git 钩子脚本。

## 项目简介

Git 默认的钩子脚本存储在 `.git/hooks` 目录中，这些文件不会被 Git 跟踪。本项目通过将钩子脚本放置在 `hooks` 目录中并纳入版本控制，实现了钩子脚本的团队共享。

## 目录结构

```
.
├── hooks/
│   └── pre-commit          # Pre-commit 钩子脚本
└── README.md               # 本文档
```

## 功能说明

### Pre-commit 钩子

当前项目包含一个 `pre-commit` 钩子，用于防止提交大文件（≥50MB）到 Git 仓库：

- 自动检测暂存区中的文件大小
- 对于 ≥50MB 的文件：
  - 自动添加到 `.gitignore`
  - 从暂存区移除
  - 阻止提交并提示用户
- 解决 GitHub 的文件大小限制问题（50MB 警告，100MB 硬限制）

## 使用方法

### 方法一：配置自定义钩子路径（推荐）

1. **克隆项目后配置钩子路径**

```bash
git config core.hooksPath hooks
```

2. **赋予钩子脚本执行权限**

```bash
cd hooks
find . -type f -exec chmod +x {} \;
```

3. **验证配置**

```bash
git config core.hooksPath
# 应该输出: hooks
```

### 方法二：使用安装脚本

1. **赋予安装脚本权限**
chmod +x install-hooks.sh

2. **运行安装脚本**
./install-hook.sh


### 方法三：手动复制

```bash
cp hooks/* .git/hooks/
chmod +x .git/hooks/*
```

## 团队协作

### 新成员加入项目

1. 克隆项目
2. 执行以下命令之一启用钩子：
   - `git config core.hooksPath hooks`（推荐）
   - 运行 `./install-hooks.sh`
   - 手动复制钩子文件

### 添加新的钩子

1. 在 `hooks` 目录中创建新的钩子脚本
2. 赋予执行权限：`chmod +x hooks/your-hook-name`
3. 提交到版本控制：
```bash
git add hooks/your-hook-name
git commit -m "Add new git hook"
git push
```

### 修改现有钩子

1. 直接编辑 `hooks` 目录中的钩子脚本
2. 提交更改：
```bash
git add hooks/
git commit -m "Update git hooks"
git push
```
3. 团队成员拉取更新后，钩子会自动生效（如果使用方法一）

## 支持的钩子类型

Git 支持多种钩子，常用的包括：

- `pre-commit` - 提交前执行
- `pre-push` - 推送前执行
- `commit-msg` - 提交信息验证
- `post-commit` - 提交后执行
- `pre-rebase` - 变基前执行

更多钩子类型请参考 [Git 官方文档](https://git-scm.com/docs/githooks)。

## 跨平台兼容性

### Python 脚本

本项目的 `pre-commit` 钩子使用 Python 3 编写，确保跨平台兼容性：

- Linux/macOS: 使用 `#!/usr/bin/env python3`
- Windows: 确保安装了 Python 3 并添加到 PATH

### Bash 脚本

如果使用 Bash 脚本，在 Windows 上需要：
- Git Bash
- WSL (Windows Subsystem for Linux)
- Cygwin

## 常见问题

### Q: 钩子没有执行？

**A:** 检查以下几点：
1. 确认钩子脚本有执行权限：`ls -l hooks/`
2. 确认配置了正确的钩子路径：`git config core.hooksPath`
3. 检查脚本的 shebang 行是否正确

### Q: 如何禁用钩子？

**A:** 临时禁用可以使用：
```bash
git commit --no-verify
```

永久禁用：
```bash
git config --unset core.hooksPath
```

### Q: 钩子在 Windows 上不工作？

**A:**
1. 确保使用 Git Bash 或 WSL
2. 对于 Python 脚本，确保安装了 Python 3
3. 检查行尾符（LF vs CRLF）：
```bash
git config core.autocrlf input
```

### Q: 如何测试钩子？

**A:** 直接运行钩子脚本：
```bash
./hooks/pre-commit
```

或者进行一次实际的 Git 操作来触发钩子。

## 注意事项

1. **权限问题**
   - 确保钩子脚本具有可执行权限
   - 在 Unix 系统上：`chmod +x hooks/*`

2. **不要覆盖本地钩子**
   - 使用 `core.hooksPath` 方式更安全
   - 避免直接覆盖 `.git/hooks` 目录

3. **钩子执行失败会阻止操作**
   - 确保钩子脚本健壮性
   - 提供清晰的错误信息
   - 必要时可以使用 `--no-verify` 跳过

4. **团队协作**
   - 在 README 或文档中说明如何启用钩子
   - 确保新成员了解钩子的作用
   - 定期审查和更新钩子脚本



## 贡献指南

欢迎提交 Issue 和 Pull Request 来改进本项目的钩子脚本。

## 参考资料

- [Git Hooks 官方文档](https://git-scm.com/docs/githooks)
- [自定义 Git - Git 钩子](https://git-scm.com/book/zh/v2/自定义-Git-Git-钩子)
- [Husky 官方文档](https://typicode.github.io/husky/)
- [pre-commit 官方文档](https://pre-commit.com/)

## 许可证

请根据项目需要添加适当的许可证。
