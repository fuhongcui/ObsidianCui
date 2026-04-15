
> [!abstract] **什么是 Stash？**
> 
> `git stash` 用于将当前工作区（Working Directory）和暂存区（Index）的修改暂时保存起来，使工作区恢复到干净状态，方便切换分支或处理紧急 Bug。

## 1. 核心命令概括

- **保存：** `git stash push` (保存当前进度)
    
- **查看：** `git stash list` (查看所有备份)
    
- **恢复：** `git stash pop` (恢复并删除) / `git stash apply` (恢复并保留)
    
- **清理：** `git stash drop` (删除单个) / `git stash clear` (清空全部)
    

---

## 2. 常用操作详解

### 🟢 保存进度 (Storing)

|**命令**|**说明**|
|---|---|
|`git stash`|快速保存（默认备注为最后一次提交信息）|
|`git stash push -m "说明文字"`|**推荐做法**，添加自定义备注|
|`git stash -u`|包含未跟踪文件 (`--include-untracked`)|
|`git stash -a`|包含所有文件（连同被忽略的 `.gitignore` 文件）|

### 🔵 查看与对比 (Inspecting)

|**命令**|**说明**|
|---|---|
|`git stash list`|列出堆栈中所有的 stash 记录|
|`git stash show`|显示最近一次 stash 的文件差异统计|
|`git stash show -p`|显示最近一次 stash 的具体代码改动内容|
|`git stash show stash@{n} -p`|查看指定编号的具体改动|

### 🟡 恢复现场 (Restoring)

> [!tip] **pop vs apply**
> 
> - `pop` = 恢复 + 删除记录（最常用）
>     
> - `apply` = 只恢复 + 保留记录（更安全）
>     

|**命令**|**说明**|
|---|---|
|`git stash pop`|恢复最近一次 stash 并从列表中移除|
|`git stash apply`|恢复最近一次 stash 但保留记录|
|`git stash apply stash@{n}`|恢复指定的 stash 记录|

### 🔴 删除与清理 (Cleaning)

|**命令**|**说明**|
|---|---|
|`git stash drop stash@{n}`|删除指定的 stash 记录|
|`git stash clear`|**慎用！** 清空堆栈中所有的 stash 记录|

---

## 3. 高级技巧

### 从 Stash 创建分支

如果你发现恢复 stash 后代码冲突太多，可以新开一个分支来承接这些修改：

Bash

```
git stash branch <新分支名> stash@{n}
```

### 仅 Stash 部分文件

如果你只想保存部分改动，而不影响其他修改：

Bash

```
git stash push -p  # 进入交互模式，逐个确认要 stash 的代码块
```

---

## 4. 常见问题 (FAQ)

> [!info] **为什么我的新文件没有被 Stash？**
> 
> 默认情况下，Git 只会 stash 已经被跟踪（Tracked）的文件。如果是新创建的文件，请务必使用 `git stash -u`。

> [!warning] **Stash Pop 冲突了怎么办？**
> 
> 如果发生冲突，Git 会保留该 stash 记录。你需要手动解决冲突，确认无误后手动执行 `git stash drop` 来清理掉记录。

---

#Git #Workflow #Notes