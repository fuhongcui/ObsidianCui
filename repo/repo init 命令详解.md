
---
## 🤖 Assistant

`repo init` 是 Repo 工具（用于管理多个 Git 仓库，常用于 Android 源码开发）的第一步命令，用于在当前目录初始化 Client。

以下是 `repo init` 最常用和重要的参数解释：

### 基本语法
```bash
repo init [options]
```

### 常用参数列表

#### 1. 必需或核心参数
*   **`-u <URL>`** (或 `--manifest-url`)
    *   **描述**：指定 Manifest 仓库的 Git URL。
    *   **说明**：这是唯一一个“几乎总是必须”的参数。Manifest 仓库包含了所有其他子项目的列表和配置信息。
    *   *示例*：`-u https://android.googlesource.com/platform/manifest`

*   **`-b <REVISION>`** (或 `--manifest-branch`)
    *   **描述**：指定 Manifest 仓库中的分支或修订版本。
    *   **说明**：如果不指定，默认是 `master` 分支。
    *   *示例*：`-b main` 或 `-b android-12.0.0_r1`

*   **`-m <NAME>`** (或 `--manifest-name`)
    *   **描述**：指定 Manifest 仓库中具体的清单文件名。
    *   **说明**：默认通常是 `default.xml`。如果仓库中有多个 XML 文件（如 `default.xml`, `full.xml`, `minimal.xml`），可以用此指定。
    *   *示例*：`-m full.xml`

#### 2. 网络与同步参数
*   **`--repo-url <URL>`**
    *   **描述**：指定 Repo 工具本身的 Git 仓库 URL。
    *   **说明**：默认通常是 Google 的源。如果你需要使用内部镜像或加速源，可以修改此项。
    *   *示例*：`--repo-url https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/`

*   **`--repo-branch <BRANCH>`**
    *   **描述**：指定 checkout 出来的 Repo 工具所使用的分支。
    *   **说明**：默认是 `stable`。

*   **`--no-repo-verify`**
    *   **描述**：不验证 Repo 源代码的标记。
    *   **说明**：如果你下载 Repo 的 URL 不是官方源，且没有相应的 GPG 标签验证，通常需要加上此参数以避免报错。

*   **`--no-clone-bundle`**
    *   **描述**：禁用下载 clone.bundle 文件。
    *   **说明**：Google 提供的 clone bundle 是为了加速某些大型仓库的克隆。但在某些网络环境下，下载 bundle 可能会失败或更慢，加上此参数可以跳过它，直接 clone。

#### 3. 性能与存储参数
*   **`--reference <PATH>`**
    *   **描述**：指定一个本地已有镜像的路径。
    *   **说明**：用于节省网络带宽和磁盘空间。如果你以前下载过一套完整的代码，或者公司有统一的代码镜像，指向那个目录，新 init 的目录会通过硬链接或对象引用共享 git 对象，极大加快 `repo sync` 的速度。
    *   *示例*：`--reference /path/to/existing/source/code`

*   **`--depth <DEPTH>`**
    *   **描述**：创建一个浅克隆。
    *   **说明**：指定深度为 1（`--depth 1`），意味着只下载最近的一次提交历史。这会极大地减少下载时间和磁盘占用，但无法查看完整的历史记录。适合 CI/CD 或仅用于编译的场景。

*   **`--partial-clone`**
    *   **描述**：启用 Git 的部分克隆功能。
    *   **说明**：允许按需下载对象，而不是一次性下载整个仓库历史。适合大仓库管理和节省带宽。

*   **`--worktree`**
    *   **描述**：使用 Git worktree 机制。
    *   **说明**：启用后，不同的分支会在不同的子目录中，而不是互相覆盖。这在管理多个构建分支时非常有用。

*   **`--archive`** (现在较少使用)
    *   **描述**：比 `--reference` 更激进的共享模式，使用 tar 等归档方式。

#### 4. 其他参数
*   **`-g <GROUP>`** (或 `--groups`)
    *   **描述**：限定要同步的 manifest 组。
    *   **说明**：在 `default.xml` 中通常定义了 `default`, `linux`, `darwin`, `notdefault` 等组。你可以用逗号分隔来指定。常用 `-g all,-notdefault` 来排除某些组。
    *   *示例*：`-g default,cfw`

*   **`-c <NAME>`** (或 `--current-branch`)
    *   **描述**：只在当前分支上操作。
    *   **说明**：相当于执行 `repo init` 后，让所有子项目都停留在 manifest 仓库里指定的那个分支上，而不是通常的 detached HEAD 状态。

*   **`--config <NAME>`**
    *   **描述**：在初始化后运行 `git config` 设置。
    *   *示例*：`--config core.filemode=false`

---

### 典型使用示例

**1. 初始化 Android 源码主分支：**
```bash
repo init -u https://android.googlesource.com/platform/manifest -b master
```

**2. 初始化 Android 源码，使用清华镜像加速，并仅下载最新代码（浅克隆）：**
```bash
repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b master --depth 1
```

**3. 使用本地镜像加速（基于已有的代码库）：**
```bash
repo init -u https://android.googlesource.com/platform/manifest -b master --reference /path/to/aosp-mirror
```

**4. 指定特定的 manifest 文件和组：**
```bash
repo init -u git@example.com:manifest.git -b dev -m my_manifest.xml -g default,my_driver
```


在使用 `repo init --reference` 初始化仓库后，你的本地代码库会建立一个指向“引用库”（Reference Repo）的硬链接或缓存机制。这能极大节省下载流量，但同步最新代码时需要注意引用库与当前库的**双重更新**。

要快速同步最新代码，建议遵循以下步骤：

---

### 1. 先更新引用库（Reference Repo）

这是最关键的一步。如果你的引用库本身是旧的，那么新同步的仓库依然需要从远程服务器下载大量数据。

进入你存放引用库的目录，执行同步：

Bash

```
cd /path/to/your/reference_repo
repo sync -j$(nproc) -c --no-tags
```

### 2. 在当前工程中执行快速同步

回到你当前的工作目录，使用以下组合参数来提升速度：

Bash

```
repo sync -j$(nproc) -c -d --no-tags --prune
```

**参数详解：**

- **`-j$(nproc)`**：使用多线程同步（`nproc` 会自动获取你电脑的 CPU 核心数）。
    
- **`-c` (或 `--current-branch`)**：只同步清单文件（manifest）中指定的当前分支，忽略其他冗余分支，大幅减少下载量。
    
- **`-d` (或 `--detach`)**：将项目回退到清单文件指定的固定版本，避免本地改动导致的冲突中断同步。
    
- **`--no-tags`**：不下载标签（Tags），这在大型项目（如 Android）中可以节省大量空间和时间。
    
- **`--prune`**：删除远程已经不存在的本地分支，保持工作区干净。
    

---

### 3. 如果想进一步加速（进阶技巧）

#### A. 减少 Git 历史深度

如果你不需要查看完整的历史提交记录，只想要最新的代码，可以在 `repo init` 时（或修改 `.repo/manifests`）加入 `--depth=1`：

Bash

```
repo init --reference=/path/to/ref -u <URL> -b <BRANCH> --depth=1
repo sync -c
```

#### B. 仅同步特定的项目

如果你只关心某几个模块，可以直接在 sync 后面加上项目路径，而不是同步整个大仓：

Bash

```
repo sync platform/frameworks/base platform/system/core
```

#### C. 利用网络优化

如果远程服务器在海外，确保你的终端配置了代理：

Bash

```
export http_proxy=http://127.0.0.1:端口号
export https_proxy=http://127.0.0.1:端口号
```