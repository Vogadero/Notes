# Git Worktree 学习指南与总结

## 1. 什么是 Git Worktree？

**Git Worktree** 是 Git 提供的一项功能，允许你在同一个 Git 仓库中同时管理多个工作目录（Working Tree）。

*   **核心概念**：通常情况下，一个 Git 仓库对应一个工作目录。使用 Worktree 后，你可以基于同一个仓库（共享 `.git` 目录和对象库），创建多个独立的工作目录。
*   **每个 Worktree 的特点**：拥有独立的 HEAD 指针、Index（暂存区）和工作区文件，但共享底层的版本历史。

## 2. 为什么需要 Git Worktree？（解决的痛点）

在日常开发中，我们经常面临以下场景，传统解决方法存在弊端：

| 场景 | 传统做法 | 痛点/弊端 | Worktree 优势 |
| :--- | :--- | :--- | :--- |
| **紧急 Bug 修复** | `git stash` 保存当前进度 -> 切分支 -> 修 Bug -> 切回来 -> `git stash pop` | `stash` 容易遗忘或丢失；频繁切换分支导致 IDE 重新索引、编译环境重置，耗时耗力。 | 创建新目录修 Bug，原目录保持不动，环境不丢失，无需 stash。 |
| **并行开发** | `git clone` 多个仓库 | 占用大量磁盘空间；多个仓库间同步（fetch/pull）繁琐；配置不共享。 | 共享 `.git` 库，节省空间；秒级创建；状态自动同步。 |
| **代码评审 (Code Review)** | 切换当前分支或拉取新分支 | 扰乱当前工作区；可能引入未提交的更改干扰测试。 | 为 PR 创建临时 Worktree，独立验证，用完即焚。 |
| **对比历史版本** | `git checkout` 历史提交 | 覆盖当前文件，破坏正在进行的工作。 | 在新目录检出历史版本，左右分屏对比，互不干扰。 |

## 3. 常用命令速查

### 3.1 创建 Worktree

```bash
# 基础用法：在指定路径创建一个新的 worktree，并检出指定分支
git worktree add <path> <branch>

# 示例：在上级目录创建 hotfix-1.5 文件夹，并检出 release/1.5 分支
git worktree add ../hotfix-1.5 release/1.5

# 创建新分支并建立 worktree
git worktree add -b <new-branch> <path>
# 示例：
git worktree add -b hotfix ../hotfix-branch

# 分离头指针模式（Detached HEAD）：检出特定 commit，不依赖分支（适合临时验证）
git worktree add --detach <path> <commit-hash>
```

### 3.2 查看 Worktree

```bash
# 列出当前仓库所有的 worktree 及其状态
git worktree list
```

### 3.3 删除与清理

```bash
# 删除 worktree（推荐方式，会自动删除关联目录）
git worktree remove <path>

# 强制删除（即使有未提交的修改）
git worktree remove -f <path>

# 清理无效记录（如果手动删除了文件夹，Git 记录还在，需要用此命令清理）
git worktree prune
```

## 4. 最佳实践与注意事项

### 4.1 目录结构管理
建议不要将 Worktree 散落在各处。可以在项目根目录同级或内部创建一个专门的文件夹来管理。
*   **推荐做法**：在项目根目录旁创建一个 `.wt` 或 `worktrees` 目录，将所有 Worktree 放在其中。
    ```bash
    git worktree add .wt/feature-x -b feature/x origin/main
    ```

### 4.2 避免分支冲突
*   **限制**：同一个分支不能同时在多个 Worktree 中被检出（Git 限制，为了防止 HEAD 指针错乱）。
*   **解决**：如果确实需要查看同一分支，可以使用 `--detach` 模式，或者创建不同名的临时分支。

### 4.3 依赖与构建
*   虽然代码共享，但构建产物（如 `node_modules`, `target/`）在每个 Worktree 是独立的。这意味着新建 Worktree 后通常需要重新安装依赖（`npm install` / `mvn install`）。
*   **优点**：这也意味着你可以同时测试不同版本的依赖，互不干扰。

### 4.4 Submodule 支持
*   目前 Git Worktree 对子模块（Submodule）的支持尚不完美，处理起来可能较麻烦，建议在使用 Submodule 的项目中谨慎使用。

## 5. 总结

Git Worktree 是一个“被低估”的神器，它结合了 `git stash` 的轻量和 `git clone` 的隔离性。

*   **适合人群**：需要频繁切换上下文、同时处理多任务、进行代码审查的开发者。
*   **核心价值**：**并行工作，互不干扰**。让你像拥有“分身术”一样，同时在多个分支上从容应对。

---
*整理自：CSDN、掘金、腾讯云开发者社区、博客园等技术博客。*
