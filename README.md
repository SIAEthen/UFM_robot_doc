# Obsidian + GitHub 团队文档更新教程

> 适用项目：ROS / ROS 2 移动机器人团队文档  
> GitHub 仓库：<https://github.com/SIAEthen/UFM_robot_doc>  
> 适用系统：Windows（Git Bash）/ Ubuntu  
> 目标：在 Obsidian 中编辑 Markdown 文档，通过 GitHub 安全地同步、审阅和保存历史版本。

## 1. 工作原理

Obsidian 的知识库（Vault）就是电脑上的一个文件夹，笔记以 `.md` 文件保存。Git 用于跟踪这些文件的修改，GitHub 用于共享远程仓库。

```text
GitHub 远程仓库
       ↑ git push      ↓ git pull
本地 Git 仓库（UFM_robot_doc/）
       ↕ 直接读写 Markdown 文件
     Obsidian
```

**记住三个操作：**
- `git pull`：从 GitHub 获取其他成员最新提交的文档。
- 在 **Obsidian** 中编辑：修改保存在本地，**不会自动上传**。
- `git add` + `git commit` + `git push`：记录修改并上传到 GitHub。

> 注意：Obsidian 的“仓库/Vault”和 Git 的“仓库/Repository”不是同一种功能，但可以使用**同一个文件夹**。

## 2. 首次配置（每台电脑只需一次）

### 2.1 安装软件

1. 安装 [Obsidian](https://obsidian.md/download)。
2. 安装 [Git](https://git-scm.com/downloads)（Windows 建议使用 Git Bash）。
3. 确保拥有 GitHub 仓库的访问权限。

### 2.2 设置 Git 身份

在 Git Bash 或终端执行：

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的Git提交邮箱"
```

验证：

```bash
git config --global user.name
git config --global user.email
```

邮箱可以使用 GitHub 提供的 `noreply` 隐私邮箱。这里的姓名和邮箱用于标记 Commit，不等于 GitHub 登录密码。

### 2.3 将远程文档仓库克隆到本地

如果尚未克隆：

```bash
mkdir -p ~/Documents/GitWorkspace
cd ~/Documents/GitWorkspace
git clone https://github.com/SIAEthen/UFM_robot_doc.git
cd UFM_robot_doc
```

在 Windows Git Bash 中，`~/Documents/GitWorkspace` 是当前用户的 Documents 目录下的 GitWorkspace 文件夹。

**如果此前已经克隆或配置好仓库，跳过 `git clone`，直接进入原来的仓库目录。** 不要在已有 `.git` 文件夹的目录中再次 `git clone` 或 `git init`。

核对本地仓库：

```bash
git status
git remote -v
git branch --show-current
```

应能看到 `origin` 指向 `https://github.com/SIAEthen/UFM_robot_doc.git`。记下当前分支名：可能是 `main`，也可能是 `master`；**不要盲目改名或直接推送其他分支**。

> 如果你之前建立的文件夹名叫 `UFM_robot_doc.git`，也可以继续使用它。文件夹名不影响 Git 工作；关键是内部存在 `.git` 目录且 `origin` 指向正确仓库。

### 2.4 用 Obsidian 打开现有目录

1. 打开 Obsidian。
2. 选择 **打开本地仓库 / Open folder as vault**。
3. 选中本地 **`UFM_robot_doc` 文件夹本身**，而不是内部的 `.git` 文件夹。
4. 点击打开。

这样 Obsidian 编辑的文件就位于 Git 仓库内。

## 3. 推荐目录结构

```text
UFM_robot_doc/
├── README.md
├── 01-项目概览/
├── 02-硬件配置/
├── 03-软件开发/
├── 04-开发日志/
├── 05-实验测试/
└── Templates/
```

- **项目概览**：机器人架构、团队分工和重要文档入口。
- **硬件配置**：雷达、IMU、底盘、供电、接线和标定。
- **软件开发**：Ubuntu、ROS 2、驱动、Launch、Topic、TF 和 Nav2 配置。
- **开发日志**：每天修改内容、遇到的问题、关联的 GitHub Issue。
- **实验测试**：实验条件、配置版本、评价指标和数据链接。
- **Templates**：日志和实验记录模板。

> Git 不跟踪空文件夹。可在每个文件夹内添加 `README.md` 或 `.gitkeep`，以便推送到 GitHub。

## 4. 每天更新文档：推荐的标准流程

### 第一步：开始编辑前先同步

在仓库目录打开 Git Bash：

```bash
cd ~/Documents/GitWorkspace/UFM_robot_doc
git status
git pull --ff-only
```

- 先运行 `git status`，确认工作区没有未提交的更改。
- `git pull --ff-only` 会更新本地分支，但在需要合并时停止，不会自动创建合并提交。
- 如果有未提交修改，先保存或提交它们；**不要为消除报错直接使用 `git reset --hard`**。

> 团队共享文档建议建立个人分支并通过 Pull Request 合并。第 5 节提供更安全的协作方式。

### 第二步：在 Obsidian 中新增或修改笔记

例如新增：

`04-开发日志/2026-10-14-张三.md`

```markdown
# 开发日志 - 2026-10-14

- 开发者：张三
- 机器人：UFM Robot
- 工作模块：激光雷达
- GitHub Issue：#12

## 今日工作
- 安装激光雷达 ROS 2 驱动。
- 测试 `/scan` Topic 的数据发布。

## 遇到的问题
- 首次运行时无法访问串口。
- 排查串口设备权限与设备映射后解决。

## 修改及验证
- 修改文件：`robot_bringup/config/lidar.yaml`
- 测试命令：`ros2 topic hz /scan`
- 结果：Topic 正常发布（具体频率见实验记录）。

## 下一步
- 检查雷达坐标系和 TF 发布。
```

> 示例中的设备、命令和结果仅用于展示格式，真实日志应写入实际测试信息。

### 第三步：查看变化

```bash
git status
git diff
```

如果刚创建了新文件，`git diff` 默认不会显示未跟踪文件的正文，但 `git status` 会显示它。

### 第四步：提交本地修改

建议优先提交与本次任务相关的文件，而不是无条件 `git add .`：

```bash
git add "04-开发日志/2026-10-14-张三.md"
git commit -m "docs: add lidar setup log for 2026-10-14"
```

### 第五步：上传 GitHub

如果你在仓库默认分支直接维护文档，并且团队允许直接推送：

```bash
git push
```

首次推送本地分支且尚未建立 upstream 时，可能需要：

```bash
git push -u origin "$(git branch --show-current)"
```

完成后打开 <https://github.com/SIAEthen/UFM_robot_doc>，检查文件和 Commit 是否出现。

**日常速记：**

```bash
# 编辑前：检查并同步
git status
git pull --ff-only

# 在 Obsidian 中编辑文档后：检查、提交和推送
git status
git add "具体文件路径.md"
git commit -m "docs: update robot setup notes"
git push
```

## 5. 团队协作：建议采用分支 + Pull Request

当多人共同维护硬件配置、ROS 2 软件部署教程或系统架构文档时，建议**不要所有人直接推送到 `main`**。

### 5.1 创建工作分支

以 GitHub 默认分支是 `main` 为例：

```bash
git switch main
git pull --ff-only
git switch -c docs/12-lidar-setup
```

如果远程仓库实际默认分支是 `master`，把示例中的 `main` 换成 `master`。

### 5.2 编辑并上传自己的分支

在 Obsidian 中修改文档后：

```bash
git status
git add "02-硬件配置/激光雷达配置.md"
git commit -m "docs: document lidar wiring and ROS2 setup"
git push -u origin docs/12-lidar-setup
```

### 5.3 在 GitHub 上创建 Pull Request（PR）

1. 打开 GitHub 文档仓库。
2. 找到新上传分支的 **Compare & pull request** 入口（或进入 Pull requests → New pull request）。
3. Base 选 `main`（或仓库实际默认分支），Compare 选 `docs/12-lidar-setup`。
4. 填写修改说明、关联 Issue、验证方式。
5. 请另一名团队成员检查内容，然后合并。

### 5.4 合并之后更新本地

```bash
git switch main
git pull --ff-only
```

确认已成功合并后，才可以按需删除本地工作分支：

```bash
git branch -d docs/12-lidar-setup
```

## 6. 多人同时编辑的注意事项

1. **每个人维护自己的每日开发日志**，例如 `2026-10-14-张三.md`，减少同一文件冲突。
2. **共享配置教程通过 PR 修改**，建议指定审阅人。
3. **每次开始前 `git pull`**，每次完成后及时提交。
4. **只提交需要共享的资料**，不要上传账号密码、SSH 私钥、机器人网络凭据。
5. **大型 rosbag、视频和数据集另存到 NAS/对象存储**；在 Markdown 中保存实验编号和路径即可。
6. **记录代码版本**：重要实验笔记注明机器人代码仓库的 Commit SHA、ROS 2 版本和参数文件版本。

建议在文档仓库根目录加入 `.gitignore`，例如：

```gitignore
# Obsidian: local workspace/UI state
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/cache/
.trash/

# Common OS files
.DS_Store
Thumbs.db

# Large ROS experimental recordings
*.db3
*.mcap
```

> `.obsidian/` 中其他设置是否共享由团队约定。若不想同步任何 Obsidian 设置，可以忽略整个 `.obsidian/`。已经被 Git 跟踪的文件不会因为后来写入 `.gitignore` 就自动取消跟踪。

## 7. 常见问题

### 7.1 `fatal: unable to auto-detect email address`

原因：未设置 Git 提交身份。

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的Git提交邮箱"
```

### 7.2 `error: src refspec master does not match any`

原因通常是本地还没有 Commit，或者推送的分支名错误。

```bash
git status
git branch --show-current
git log -1 --oneline
```

若是刚创建的空仓库，需要先 `git add`、`git commit`，再推送**实际分支名**。如果远程已有 README 和提交，优先克隆仓库，不要强制推送。

### 7.3 `rejected (fetch first)` 或 `non-fast-forward`

原因：远程分支有本地尚未获取的提交。

```bash
git status
git pull --ff-only
```

如果无法快进，说明需要人工处理分支分歧；先检查 `git log --oneline --graph --all -10`，不要盲目 `git push --force`。团队成员可以在单独分支上解决合并或变基问题。

### 7.4 `Your local changes would be overwritten by merge`

原因：本地有未提交的改动，Git 为避免覆盖而阻止拉取。

优先在 Obsidian 确认修改，再提交：

```bash
git status
git add "具体文件路径.md"
git commit -m "docs: save local changes"
```

然后再根据所在分支和团队流程同步远程更改；必要时使用 `git stash` 暂存，但先确认文件已安全保存。

### 7.5 出现合并冲突怎么办？

Git 可能在冲突文件中写入：

```text
<<<<<<< HEAD
本地内容
=======
远程内容
>>>>>>> other-branch
```

在 VS Code 或文本编辑器中比较并整理成最终正文，删掉冲突标记。确认无误后使用 `git add` 标记已解决，并按 Git 提示继续 `merge` / `rebase`。不熟悉时先备份冲突文档并联系负责该文件的成员。

### 7.6 HTTPS 能用吗？需要 SSH 吗？

**HTTPS 可以正常使用。** 地址 `https://github.com/SIAEthen/UFM_robot_doc.git` 是合法的 Git 远程地址。首次推送可能需要浏览器授权、Git Credential Manager 或 Personal Access Token；GitHub 不能通过普通账号密码完成 Git HTTPS 认证。不必为了解决分支或 Commit 报错而切换成 SSH。

## 8. 团队开发日志最小模板

在 `Templates/每日开发日志.md` 中保存：

```markdown
# 开发日志 - YYYY-MM-DD

**成员：**  
**模块：**  
**关联 Issue：**  
**机器人代码 Commit：**  

## 今日工作
- 

## 修改内容
- 文件 / 参数：
- 操作说明：

## 问题与解决
- 问题：
- 原因 / 排查：
- 结果：

## 测试验证
- 测试方法：
- 测试结果：
- 数据 / 实验记录链接：

## 下一步
- [ ] 
```

## 9. 完成一次更新前的检查清单

- [ ] 编辑前确认当前分支及 Git 状态，获取远程最新更改。
- [ ] 文档内容放在正确目录，名称能看出用途。
- [ ] 重要参数、命令和测试结果有来源或记录。
- [ ] 没有上传密码、私钥、敏感网络信息或大型实验文件。
- [ ] Git Commit 信息准确描述改动。
- [ ] 共享文档走 PR 审核（如果团队启用）。
- [ ] GitHub 上能够看到更新后的文档。

---

## 参考资料

- [Obsidian 官方：创建和打开知识库](https://obsidian.md/help/vault)
- [Obsidian 官方：数据如何保存](https://obsidian.md/help/data-storage)
- [GitHub 官方：克隆仓库](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)
- [GitHub 官方：获取远程更改](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository)
