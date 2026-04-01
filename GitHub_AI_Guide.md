# GitHub 使用指南

## 前言

本指南整合了 Git 基础操作、GitHub 核心功能及进阶用法，旨在为开发者提供一套逻辑清晰、内容完备的 GitHub 全流程使用手册，覆盖从环境配置到团队协作、自动化工作流的全场景，适用于新手入门与进阶提升。

## 一、Git 基础

Git 是分布式版本控制系统，是使用 GitHub 的核心基础，掌握 Git 命令是高效使用 GitHub 的前提。

### 1.1 Git 简介

Git 由 Linus Torvalds 开发，用于管理 Linux 内核源码，核心优势：

- **分布式架构**：每个开发者本地拥有完整版本库，无需依赖中央服务器（断网也可开发）；
- **高效的版本追踪**：支持快速提交、分支切换、版本回退；
- **轻量且高性能**：底层采用哈希算法（SHA-1）保证版本完整性，操作速度远快于集中式版本控制系统（如 SVN）。

### 1.2 Git 安装与配置

#### 1.2.1 安装（跨平台）

- **Windows**：
  下载官方安装包 [Git for Windows](https://git-scm.com/download/win)，按向导安装（建议勾选“Git Bash Here”方便右键调用终端）。
- **macOS**：
  方式1：通过 Homebrew 安装 `brew install git`；
  方式2：下载官方安装包 [Git for macOS](https://git-scm.com/download/mac)。
- **Linux（Ubuntu/Debian）**：
  `sudo apt update && sudo apt install git`；
  CentOS/RHEL：`sudo yum install git`。

验证安装：终端执行 `git --version`，输出版本号即安装成功。

#### 1.2.2 基础配置（必做）

Git 首次使用需配置用户信息（关联 GitHub 账号），终端执行：

```bash
# 配置全局用户名（与 GitHub 用户名一致）
git config --global user.name "YourGitHubUsername"
# 配置全局邮箱（与 GitHub 注册邮箱一致）
git config --global user.email "your_email@example.com"
# 配置默认编辑器（可选，如 VS Code）
git config --global core.editor "code --wait"
# 查看配置信息
git config --list
```

### 1.3 Git 核心概念

理解以下概念是掌握 Git 的关键：

| 概念                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| 工作区（Workspace）   | 本地电脑中实际编辑代码的目录（可见的文件/文件夹）            |
| 暂存区（Index/Stage） | 临时存储待提交的修改，是工作区与版本库的中间层（`.git/index` 文件） |
| 版本库（Repository）  | Git 管理的核心目录（`.git` 文件夹），存储所有版本记录、分支等信息 |
| 提交（Commit）        | 将暂存区的修改保存到版本库，生成唯一哈希值（commit ID）作为版本标识 |
| 分支（Branch）        | 独立的开发线，默认主分支为 `main`（旧版为 `master`），可并行开发功能 |
| 远程仓库（Remote）    | 托管在服务器的版本库（如 GitHub 上的仓库），本地仓库可与远程同步 |

### 1.4 Git 常用基础命令

#### 1.4.1 仓库初始化与克隆

- **初始化本地仓库**：在项目目录执行

  ```bash
  git init  # 生成 .git 目录，将当前目录转为 Git 仓库
  ```

- **克隆远程仓库**：将 GitHub 仓库复制到本地

  ```bash
  # 方式1：HTTPS 地址（需输入账号密码/个人访问令牌）
  git clone https://github.com/用户名/仓库名.git
  # 方式2：SSH 地址（需配置 SSH 密钥，免密登录）
  git clone git@github.com:用户名/仓库名.git
  ```

#### 1.4.2 文件暂存与提交

```bash
# 查看文件状态（未跟踪/已修改/已暂存）
git status
# 暂存单个文件
git add 文件名
# 暂存所有修改/新增文件
git add .
# 撤销暂存（单个文件）
git reset 文件名
# 提交暂存区到版本库（必须写提交信息）
git commit -m "feat: 新增用户登录功能"
# 补充提交（修改最后一次提交，不新增 commit ID）
git commit --amend -m "feat: 完善用户登录功能"
```

#### 1.4.3 分支操作

```bash
# 查看本地分支（* 标记当前分支）
git branch
# 查看远程分支
git branch -r
# 查看所有分支（本地+远程）
git branch -a
# 创建新分支
git branch 分支名
# 切换分支
git checkout 分支名
# 创建并切换分支（常用快捷方式）
git checkout -b 分支名
# 删除本地分支（需先切换到其他分支）
git branch -d 分支名
# 强制删除未合并的本地分支
git branch -D 分支名
# 删除远程分支
git push origin --delete 分支名
```

#### 1.4.4 远程仓库交互

```bash
# 关联本地仓库到远程仓库
git remote add origin 远程仓库地址
# 查看已关联的远程仓库
git remote -v
# 拉取远程仓库最新代码（合并到当前分支）
git pull origin 分支名
# 推送本地分支到远程仓库
git push origin 分支名
# 首次推送时关联本地分支与远程分支（后续可直接 git push）
git push -u origin 分支名
# 移除已关联的远程仓库
git remote remove origin
```

#### 1.4.5 版本回退与撤销

```bash
# 查看提交历史（含 commit ID、作者、时间、信息）
git log  # 精简版：git log --oneline
# 回退到指定版本（保留工作区修改）
git reset --soft commit ID
# 回退到指定版本（重置暂存区，保留工作区）
git reset --mixed commit ID  # 默认模式
# 强制回退到指定版本（清空工作区/暂存区，谨慎使用）
git reset --hard commit ID
# 撤销工作区的修改（未暂存的文件）
git checkout -- 文件名
# 恢复已删除的文件（从版本库恢复）
git checkout commit ID -- 文件名
```

## 二、GitHub 核心操作

GitHub 是基于 Git 的代码托管平台，提供网页端操作、协作工具、静态部署等核心功能，是开源协作的核心载体。

### 2.1 GitHub 账号与仓库管理

#### 2.1.1 账号注册与登录

- 访问 [GitHub 官网](https://github.com)，填写用户名、邮箱、密码完成注册；
- 登录后建议完成邮箱验证（避免功能限制），并完善个人资料（头像、简介等）。

#### 2.1.2 创建/删除/重命名仓库

##### 创建仓库

1. 点击页面右上角 `+` → `New repository`；
2. 填写核心信息：
   - Repository name：仓库名（唯一，建议小写+连字符，如 `vue-admin-template`）；
   - Description：仓库描述（可选，说明仓库用途）；
   - Public/Private：公有（所有人可见）/私有（仅自己/协作者可见）；
   - 可选初始化项：`Add a README file`（仓库说明）、`Add .gitignore`（忽略文件配置）、`Choose a license`（开源许可证）；
3. 点击 `Create repository` 完成创建。

##### 删除/重命名仓库

- **重命名**：进入仓库 → `Settings` → `Repository name` 处修改 → `Rename`；
- **删除**：进入仓库 → `Settings` → 拉到最底部 `Danger Zone` → `Delete this repository` → 输入仓库名确认删除（谨慎操作，删除后不可恢复）。

#### 2.1.3 仓库权限设置

- **公有仓库**：默认所有人可查看，仅仓库所有者/协作者可修改；
- **私有仓库**：
  1. 新增协作者：仓库 → `Settings` → `Collaborators` → `Add people`，输入 GitHub 用户名/邮箱；
  2. 权限分级：可设置 `Read`（只读）、`Write`（可写）、`Admin`（管理员）；
- **组织级权限**：适合团队管理，可创建 Organization，按团队分配仓库权限（进阶功能）。

### 2.2 GitHub 网页端操作

#### 2.2.1 文件的增删改查

- **新增文件**：仓库主页 → `Add file` → `Create new file`，填写文件名、内容，提交时需写 commit 信息；
- **修改文件**：点击文件 → 右上角铅笔图标（Edit），修改后提交；
- **删除文件**：点击文件 → 右上角垃圾桶图标（Delete），确认后提交；
- **查看文件历史**：点击文件 → `History`，可查看所有版本修改记录。

#### 2.2.2 Issue 管理

Issue 用于追踪 bug、提出需求、讨论问题，是协作沟通的核心工具：

1. **创建 Issue**：仓库 → `Issues` → `New issue`，填写标题、描述（支持 Markdown），可添加标签（Label）、分配负责人（Assignee）、关联里程碑（Milestone）；
2. **管理 Issue**：
   - 标签（Label）：自定义分类（如 `bug`、`feature`、`docs`），便于筛选；
   - 评论（Comment）：在 Issue 下回复讨论，支持 @用户名 提醒相关人；
   - 关闭 Issue：问题解决后点击 `Close issue`，可关联 commit/PR 自动关闭（如提交信息写 `fix #123`，合并后自动关闭 #123 Issue）。

#### 2.2.3 Pull Request (PR) 流程

PR 是开源协作的核心，用于向仓库提交代码修改（尤其是 Fork 后的仓库）：

1. **发起 PR**：
   - 先 Fork 目标仓库到自己账号 → 克隆到本地 → 创建分支开发 → 推送分支到自己的 Fork 仓库；
   - 目标仓库主页 → `Pull requests` → `New pull request` → 选择自己的 Fork 仓库和分支 → 填写 PR 标题/描述 → `Create pull request`；
2. **PR 评审**：
   - 仓库维护者可查看 PR 代码、提出修改建议（Review）；
   - 提交者可根据评审意见修改代码，推送后 PR 会自动更新；
3. **合并 PR**：评审通过后，维护者点击 `Merge pull request`，选择合并方式（Squash/Merge/Rebase），完成后可删除分支。

#### 2.2.4 GitHub Pages 部署静态页面

GitHub Pages 支持免费部署静态网站（HTML/CSS/JS/Vue/React 打包产物等）：

1. 仓库要求：
   - 仓库名建议：`用户名.github.io`（默认域名访问）；
   - 或普通仓库，通过分支部署（如 `gh-pages` 分支）；
2. 部署步骤：
   - 仓库 → `Settings` → `Pages`；
   - `Build and deployment` → `Source` 选择部署来源（如 `Deploy from a branch`）；
   - 选择分支（如 `main`）和目录（如 `/root`）→ `Save`；
   - 等待部署完成，访问提示的域名（如 `https://用户名.github.io/仓库名`）即可。

### 2.3 GitHub 协作与沟通

#### 2.3.1 Fork 与 Pull Request 协作模式

适用于贡献开源项目（无直接仓库权限）：

1. Fork 目标仓库到个人账号；
2. 克隆个人 Fork 仓库到本地，创建功能分支开发；
3. 推送分支到个人 Fork 仓库，向原仓库发起 PR；
4. 原仓库维护者评审后合并，完成贡献。

#### 2.3.2 Review PR 代码

1. 进入 PR 页面 → `Files changed`，可逐行查看代码修改；
2. 对单行文法添加评论（点击行号旁 `+`），提出修改建议；
3. 整体评审：点击 `Review changes`，选择 `Approve`（通过）、`Request changes`（要求修改）、`Comment`（仅评论）。

#### 2.3.3 GitHub Discussions 与 Comments

- **Discussions**：仓库的论坛功能，用于非 Issue 类的讨论（如需求征集、使用答疑），仓库 → `Discussions` → `New discussion`；
- **Comments**：在 Issue/PR/Commit 下的评论，支持 Markdown、代码块、表情，可 @ 团队成员触发提醒。

## 三、Git & GitHub 进阶用法

### 3.1 Git 分支管理策略

#### 3.1.1 Git Flow 规范

适用于大型项目/迭代式开发，核心分支：

- `main`：生产环境分支，仅存放稳定版本，禁止直接提交；
- `develop`：开发主分支，所有功能分支从该分支创建；
- `feature/*`：功能分支（如 `feature/user-login`），从 `develop` 创建，完成后合并回 `develop`；
- `release/*`：发布分支（如 `release/v1.0`），从 `develop` 创建，测试通过后合并到 `main` 和 `develop`；
- `hotfix/*`：紧急修复分支（如 `hotfix/v1.0.1`），从 `main` 创建，修复后合并到 `main` 和 `develop`。

#### 3.1.2 GitHub Flow 规范

适用于轻量项目/持续部署，极简流程：

1. 从 `main` 分支创建功能分支（如 `feature/xxx`）；
2. 分支上开发并提交代码，推送至远程；
3. 发起 PR，评审通过后合并回 `main`；
4. 合并后删除功能分支，`main` 分支始终可部署。

### 3.2 Git 高级命令

#### 3.2.1 Rebase 与 Merge 对比

- **Merge**：合并分支时保留所有 commit 记录，生成新的合并 commit，优点：保留历史脉络，缺点：commit 记录杂乱；

  ```bash
  git checkout main
  git merge feature/xxx  # 合并 feature/xxx 到 main
  ```

- **Rebase**：变基操作，将当前分支的 commit 移植到目标分支顶端，优点：commit 记录线性整洁，缺点：修改历史（协作分支慎用）；

  ```bash
  git checkout feature/xxx
  git rebase main  # 将 feature/xxx 变基到 main 顶端
  ```

> 注意：公共分支（如 `main`/`develop`）禁止执行 rebase，避免打乱他人协作记录。

#### 3.2.2 Cherry-pick 挑选提交

将指定 commit ID 的修改复制到当前分支（适用于跨分支复用单个提交）：

```bash
# 查看目标 commit ID
git log --oneline
# 挑选该 commit 到当前分支
git cherry-pick commit ID
```

#### 3.2.3 Stash 暂存工作区

临时保存未提交的修改（适用于切换分支前未完成开发的场景）：

```bash
# 暂存工作区所有修改
git stash
# 查看暂存列表
git stash list
# 恢复最新暂存（保留暂存记录）
git stash apply
# 恢复最新暂存并删除暂存记录
git stash pop
# 删除指定暂存
git stash drop stash@{0}
# 清空所有暂存
git stash clear
```

#### 3.2.4 Tag 标签管理（发布版本）

Tag 用于标记重要版本（如 v1.0.0），便于版本回溯：

```bash
# 创建轻量标签（仅记录 commit ID）
git tag v1.0.0
# 创建附注标签（含详细信息，推荐）
git tag -a v1.0.0 -m "v1.0.0 正式发布"
# 查看所有标签
git tag
# 推送标签到远程仓库
git push origin v1.0.0
# 推送所有标签到远程
git push origin --tags
# 检出标签对应的版本
git checkout v1.0.0
```

### 3.3 GitHub 高级功能

#### 3.3.1 GitHub Actions 自动化工作流

GitHub Actions 可实现 CI/CD（持续集成/持续部署），通过 `.github/workflows/` 目录下的 YAML 文件配置：
示例：Node.js 项目自动测试+部署 GitHub Pages

```yaml
name: Test & Deploy
on:
  push:
    branches: [main]  # 触发条件：main 分支推送代码
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
  deploy:
    needs: test  # 依赖 test 任务完成
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build
        run: npm install && npm run build
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

#### 3.3.2 Git Submodule 子模块

用于在主仓库中引入其他仓库（如公共组件库），保持独立版本管理：

```bash
# 添加子模块
git submodule add 子仓库地址 本地目录
# 克隆包含子模块的仓库（需递归克隆）
git clone --recursive 主仓库地址
# 更新子模块到最新版本
git submodule update --remote
```

#### 3.3.3 GitHub API 基础使用

GitHub 提供 REST API 和 GraphQL API，可通过接口操作仓库、Issue、PR 等：
示例：通过 REST API 获取仓库信息（curl 命令）

```bash
curl -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/用户名/仓库名
```

> 需认证时，添加 `-H "Authorization: token 你的个人访问令牌"`（令牌生成：GitHub → Settings → Developer settings → Personal access tokens）。

#### 3.3.4 代码扫描与漏洞检测

- **GitHub CodeQL**：仓库 → `Security` → `Code scanning`，启用 CodeQL 分析，自动检测代码漏洞；
- **Dependabot**：自动检测依赖包漏洞，仓库 → `Settings` → `Code security and analysis` → 启用 `Dependabot alerts` 和 `Dependabot security updates`，自动生成 PR 修复依赖漏洞。

## 四、常见问题与解决方案

### 4.1 Git 常见报错与解决

- **报错1：`fatal: remote origin already exists`**
  原因：本地仓库已关联远程 origin，解决：先移除再关联 `git remote remove origin && git remote add origin 新地址`；
- **报错2：`fatal: Authentication failed`**
  原因：HTTPS 方式认证失败，解决：改用 SSH 方式，或生成个人访问令牌（PAT）替代密码；
- **报错3：`error: Your local changes to the following files would be overwritten by merge`**
  原因：本地修改与远程冲突，解决：暂存本地修改 `git stash` → 拉取代码 `git pull` → 恢复暂存 `git stash pop` → 解决冲突。

### 4.2 GitHub 访问问题

- **国内访问缓慢/无法访问**：
  方案1：配置 GitHub 加速域名（修改 hosts 文件）；
  方案2：使用代理工具；
  方案3：通过 GitHub 镜像站（如 `https://github.com.cnpmjs.org`）克隆仓库。

### 4.3 分支冲突解决

冲突是多分支开发的常见问题，解决步骤：

1. 拉取远程最新代码 `git pull origin 分支名`，触发冲突提示；
2. 打开冲突文件，查看冲突标记（`<<<<<<< HEAD` 到 `=======` 是本地代码，`=======` 到 `>>>>>>> 分支名` 是远程代码）；
3. 手动修改冲突内容（保留正确逻辑），删除冲突标记；
4. 暂存修改 `git add 冲突文件` → 提交 `git commit -m "fix: 解决分支冲突"` → 推送代码 `git push origin 分支名`。

## 五、总结与拓展资源

### 总结

本指南覆盖了 Git 基础命令、GitHub 核心操作、进阶协作策略及自动化工具，核心原则：

1. 遵循分支规范，避免直接操作主分支；
2. 提交信息清晰（推荐规范：`类型: 描述`，如 `fix: 修复登录验证码失效问题`）；
3. 开源协作优先使用 Fork + PR 模式，重视代码评审。

### 拓展资源

- **官方文档**：
  - Git 官方文档：https://git-scm.com/doc
  - GitHub 官方文档：https://docs.github.com
- **学习工具**：
  - Git 可视化学习：https://learngitbranching.js.org
  - GitHub 技能训练：https://lab.github.com
- **规范参考**：
  - Conventional Commits（提交信息规范）：https://www.conventionalcommits.org
  - Semantic Versioning（版本号规范）：https://semver.org