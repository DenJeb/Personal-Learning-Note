# Ubuntu Linux 命令

> 本文按主题分类整理 Ubuntu 常用命令，每条命令包含作用、用法示例和选项解释。
> 建议配合终端实操。遇到不熟悉的命令，优先使用 `man 命令` 或 `whatis 命令` 查询官方手册。
> 标记 ⚠️ 的用法有风险，执行前请确认命令无误。

## 目录

- [1. 前置知识：Linux 目录结构](#basics)
- [2. 文件与目录操作](#file-ops)
  - [2.1 ls（list）](#cmd-ls)
  - [2.2 cd（change directory）](#cmd-cd)
  - [2.3 pwd（print working directory）](#cmd-pwd)
  - [2.4 touch（创建空文件）](#cmd-touch)
  - [2.5 mkdir（make directory）](#cmd-mkdir)
  - [2.6 rmdir（remove directory）](#cmd-rmdir)
  - [2.7 rm（remove）](#cmd-rm)
  - [2.8 cp（copy）](#cmd-cp)
  - [2.9 mv（move）](#cmd-mv)
  - [2.10 find（按条件查找文件）](#cmd-find)
- [3. 文件查看与编辑](#view)
  - [3.1 cat（concatenate）](#cmd-cat)
  - [3.2 less（分页查看文件）](#cmd-less)
  - [3.3 head（查看文件开头）](#cmd-head)
  - [3.4 tail（查看文件末尾）](#cmd-tail)
  - [3.5 wc（word count）](#cmd-wc)
  - [3.6 grep（global regular expression print）](#cmd-grep)
  - [3.7 nano（终端文本编辑器）](#cmd-nano)
- [4. 权限与用户管理](#permissions)
  - [4.1 chmod（change mode）](#cmd-chmod)
  - [4.2 sudo（super user do）](#cmd-sudo)
  - [4.3 useradd（user add）](#cmd-useradd)
  - [4.4 userdel（user delete）](#cmd-userdel)
  - [4.5 用户组管理（groupadd / groupdel / usermod）](#cmd-group)
- [5. 系统资源与进程](#system)
  - [5.1 top（实时进程监控）](#cmd-top)
  - [5.2 ps（process status）](#cmd-ps)
  - [5.3 kill（向进程发送信号）](#cmd-kill)
  - [5.4 free（内存使用情况）](#cmd-free)
  - [5.5 df（disk free）](#cmd-df)
  - [5.6 du（disk usage）](#cmd-du)
- [6. 软件管理](#packages)
  - [6.1 apt（Advanced Package Tool）](#cmd-apt)
- [7. 网络](#network)
  - [7.1 ifconfig 与 ip（网络接口）](#cmd-ifconfig)
  - [7.2 netstat 与 ss（网络连接）](#cmd-netstat)
- [8. 压缩与归档](#archive)
  - [8.1 tar（tape archive）](#cmd-tar)
- [9. Shell 基础](#shell)
  - [9.1 echo（输出内容）](#cmd-echo)
  - [9.2 .bashrc（bash run commands）](#cmd-bashrc)
  - [9.3 Shell 脚本（.sh）](#cmd-shell-script)
  - [9.4 同时执行多个命令（; && || |）](#cmd-multi)
- [10. 系统信息与帮助](#info)
  - [10.1 date（查看与设置时间）](#cmd-date)
  - [10.2 cal（calendar）](#cmd-cal)
  - [10.3 which（查找命令位置）](#cmd-which)
  - [10.4 whatis（命令简介）](#cmd-whatis)
  - [10.5 man（manual）](#cmd-man)
  - [10.6 clear（清空终端）](#cmd-clear)

---

## 1. 前置知识：Linux 目录结构 <a id="basics"></a>

使用命令之前，先了解 Linux 的目录结构。Linux 只有一个根目录 `/`，所有文件都挂在它下面：

```text
/
├── home/      每个用户的主目录（~ 指向当前用户的主目录，如 /home/user）
├── etc/       系统配置文件（如 /etc/hosts、/etc/group）
├── usr/       用户安装的软件和共享数据（如 /usr/bin/ls）
├── var/       经常变化的数据（如 /var/log 日志）
├── tmp/       临时文件
├── bin/       基础命令（实际是 /usr/bin 的链接）
├── sbin/      系统管理命令
├── root/      root 用户的主目录
├── dev/       设备文件
└── proc/      系统运行信息（虚拟目录）
```

**路径符号**

| 符号 | 含义 |
|---|---|
| `.` | 当前目录 |
| `..` | 上一级目录 |
| `~` | 当前用户主目录 |
| `/` | 根目录 |

- 绝对路径：从 `/` 开始的完整路径，如 `/home/user/Desktop`
- 相对路径：从当前目录开始的路径，如 `Desktop`、`../Desktop`

---

## 2. 文件与目录操作 <a id="file-ops"></a>

### 2.1 ls（list） <a id="cmd-ls"></a>

**作用**

> 查看当前目录下的文件和目录。

**用法**

```bash
# 查看当前目录内容
ls
```

```bash
# 以详细信息显示文件
ls -l
```

```bash
# 显示隐藏文件（以 . 开头的文件）
ls -a
```

```bash
# 显示所有文件，并查看详细信息
ls -la
```

```bash
# 以易读方式显示文件大小（如 4.0K 而不是 4096）
ls -lh
```

```bash
# 按修改时间排序（最新的在最前）
ls -lt
```

```bash
# 按文件大小排序
ls -lS
```

**解释**

- `ls` = list
- `-l` = long，显示详细信息（权限、大小、时间等）
- `-a` = all，显示隐藏文件
- `-h` = human readable，以 K/M/G 显示大小（需配合 `-l`）
- `-t` = time，按修改时间排序
- `-S` = size，按文件大小排序

---

### 2.2 cd（change directory） <a id="cmd-cd"></a>

**作用**

> 切换当前所在目录。

**用法**

```bash
# 进入指定目录（相对路径）
cd 目录名
```

例如：

```bash
# 进入 Desktop 文件夹
cd Desktop
```

```bash
# 进入指定目录（绝对路径）
cd /home/user/Desktop
```

```bash
# 返回上一级目录
cd ..
```

```bash
# 返回用户主目录
cd ~
```

```bash
# 进入根目录
cd /
```

```bash
# 返回上一次所在的目录（在两个目录间快速切换）
cd -
```

**解释**

- `cd` = change directory
- `..` 表示上一级目录
- `~` 表示当前用户主目录
- `/` 表示根目录
- `-` 表示上一次所在的目录

---

### 2.3 pwd（print working directory） <a id="cmd-pwd"></a>

**作用**

> 显示当前所在目录的绝对路径。

**用法**

```bash
# 查看当前路径
pwd
```

输出示例：

```text
/home/user/Desktop
```

**解释**

- `pwd` = print working directory
- 切换目录后不确定自己在哪里时，先用 pwd 确认

---

### 2.4 touch（创建空文件） <a id="cmd-touch"></a>

**作用**

> 创建空文件；如果文件已存在，则更新其修改时间。

**用法**

```bash
# 创建空文件
touch 文件名
```

例如：

```bash
# 创建 hello.txt 文件
touch hello.txt
```

```bash
# 同时创建多个文件
touch a.txt b.txt
```

```bash
# 创建指定路径下的文件
touch notes/linux.md
```

**解释**

- `touch` = 触碰
- 文件不存在时创建空文件
- 文件已存在时更新文件的修改时间（不改变内容）

---

### 2.5 mkdir（make directory） <a id="cmd-mkdir"></a>

**作用**

> 创建目录（文件夹）。

**用法**

```bash
# 创建目录
mkdir 文件夹名
```

例如：

```bash
# 创建 project 文件夹
mkdir project
```

```bash
# 创建多个目录
mkdir test1 test2
```

```bash
# 创建多级目录
mkdir -p project/src
```

创建：

```text
project
└── src
```

如果父目录不存在，会自动创建。

```bash
# 创建目录并显示创建过程
mkdir -pv project/src/test
```

**解释**

- `mkdir` = make directory
- `-p` = parents，自动创建不存在的父目录
- `-v` = verbose，显示创建过程

---

### 2.6 rmdir（remove directory） <a id="cmd-rmdir"></a>

**作用**

> 删除空目录。

**用法**

```bash
# 删除空目录
rmdir 目录名
```

例如：

```bash
# 删除 test 文件夹（空目录）
rmdir test
```

```bash
# 删除多个空目录
rmdir test1 test2
```

**解释**

- `rmdir` = remove directory
- 只能删除空目录，目录中存在文件或其他目录时删除会失败
- 实际使用中，删除非空目录更常用 `rm -r`（见 rm 一节）

---

### 2.7 rm（remove） <a id="cmd-rm"></a>

**作用**

> 删除文件或目录。

**用法**

```bash
# 删除文件
rm 文件名
```

例如：

```bash
# 删除 hello.txt 文件
rm hello.txt
```

```bash
# 删除前询问确认
rm -i 文件名
```

```bash
# 递归删除目录
rm -r 目录名
```

例如：

```bash
# 删除 project 文件夹以及里面的内容
rm -r project
```

```bash
# 删除文件并显示过程
rm -v 文件名
```

```bash
# 递归删除并显示过程
rm -rv 目录名
```

```bash
# 强制删除文件
rm -f 文件名
```

```bash
# 强制递归删除目录
rm -rf 目录名
```

**解释**

- `rm` = remove
- `-r` = recursive，递归删除目录及里面所有内容
- `-v` = verbose，显示删除过程
- `-i` = interactive，删除前询问确认（建议养成使用习惯）
- `-f` = force，强制删除，不询问
- Linux 没有回收站，删除后通常无法恢复

> ⚠️ **注意**：`rm -rf` 非常危险，执行前务必确认路径正确，千万不要对 `/` 或 `~` 误操作。

---

### 2.8 cp（copy） <a id="cmd-cp"></a>

**作用**

> 复制文件或目录。

**用法**

```bash
# 复制文件
cp 源文件 目标文件
```

例如：

```bash
# 复制 test.txt 为 backup.txt
cp test.txt backup.txt
```

```bash
# 复制文件到指定目录
cp 文件名 目录
```

例如：

```bash
# 将 test.txt 复制到 Desktop
cp test.txt Desktop
```

```bash
# 覆盖前询问
cp -i 源文件 目标
```

```bash
# 不覆盖已存在的文件
cp -n 源文件 目标
```

```bash
# 递归复制目录
cp -r 源目录 目标目录
```

例如：

```bash
# 复制 project 文件夹到 backup
cp -r project backup
```

**解释**

- `cp` = copy
- `-r` = recursive，递归复制目录及里面所有内容
- `-i` = interactive，覆盖前询问
- `-n` = no-clobber，不覆盖已存在的文件
- 默认情况下，目标文件已存在会被直接覆盖，建议配合 `-i` 使用

---

### 2.9 mv（move） <a id="cmd-mv"></a>

**作用**

> 移动文件或目录，也可以用于重命名。

**用法**

```bash
# 移动文件
mv 源文件 目标位置
```

例如：

```bash
# 将 test.txt 移动到 Desktop
mv test.txt Desktop
```

```bash
# 移动目录
mv 源目录 目标位置
```

```bash
# 重命名文件
mv 原文件名 新文件名
```

例如：

```bash
# 将 old.txt 改名为 new.txt
mv old.txt new.txt
```

```bash
# 覆盖前询问
mv -i 源文件 目标位置
```

```bash
# 不覆盖已存在的文件
mv -n 源文件 目标位置
```

```bash
# 强制覆盖，不询问
mv -f 源文件 目标位置
```

**解释**

- `mv` = move
- 可以实现文件/目录的移动和重命名
- `-i` = interactive，覆盖前询问
- `-n` = no-clobber，不覆盖已存在的文件
- `-f` = force，强制覆盖
- 默认情况下，目标文件已存在会被直接覆盖，建议配合 `-i` 使用

---

### 2.10 find（按条件查找文件） <a id="cmd-find"></a>

**作用**

> 按名称、类型、大小、时间等条件查找文件或目录。

**用法**

```bash
# 按名称查找
find 路径 -name 文件名
```

例如：

```bash
# 在当前目录查找 test.txt
find . -name test.txt
```

```bash
# 使用通配符查找（通配符要加引号，避免被 Shell 展开）
find . -name "*.txt"
```

```bash
# 限制查找深度（只查找当前目录，不进入子目录）
find . -maxdepth 1 -name "*.txt"
```

```bash
# 按类型查找
find 路径 -type 类型
```

例如：

```bash
# 查找所有目录
find . -type d
```

```bash
# 查找所有文件
find . -type f
```

```bash
# 按大小查找
find 路径 -size 大小
```

例如：

```bash
# 查找大于 100MB 的文件
find . -size +100M
```

```bash
# 按修改时间查找（最近 7 天内修改过的文件）
find . -mtime -7
```

```bash
# 对找到的每个文件执行命令
find . -name "*.log" -exec ls -lh {} \;
```

```bash
# 直接删除找到的文件
find . -name "*.tmp" -delete
```

**解释**

- `.` = 当前目录
- `-name` = 按名称查找（支持通配符，记得加引号）
- `-type` = 按类型查找：`f` 文件、`d` 目录
- `-size` = 按大小查找：`+100M` 大于、`-1M` 小于
- `-mtime` = 按修改时间查找：`-7` 最近 7 天内、`+7` 超过 7 天
- `-maxdepth N` = 最多向下查找 N 层目录
- `-exec 命令 {} \;` = 对每个结果执行命令，`{}` 代表找到的文件
- `-delete` = 删除找到的文件，删除前先用不含 -delete 的命令确认结果

---

## 3. 文件查看与编辑 <a id="view"></a>

### 3.1 cat（concatenate） <a id="cmd-cat"></a>

**作用**

> 查看文件内容（适合小文件）。

**用法**

```bash
cat 文件名
```

例如：

```bash
# 查看 hello.txt 文件内容
cat hello.txt
```

**其他用法**

创建或覆盖文件：

```bash
cat > test.txt
```

输入内容后按：

```bash
Ctrl + D
```

保存退出。

追加内容：

```bash
cat >> test.txt
```

**解释**

- `cat` = concatenate，本意是拼接文件
- 常用于快速查看文本文件内容
- `>` 会覆盖已有内容，`>>` 是追加
- 文件较大时用 less 更合适（cat 会一次性输出全部内容）

> ⚠️ **注意**：`cat > 文件` 会覆盖已有文件内容，操作前确认文件名无误。

---

### 3.2 less（分页查看文件） <a id="cmd-less"></a>

**作用**

> 分页查看文件内容，适合查看较大的文本文件。

**用法**

```bash
# 查看文件内容
less 文件名
```

例如：

```bash
# 查看 log.txt 文件
less log.txt
```

**less 中常用操作**

| 按键 | 作用 |
|---|---|
| `↑` / `↓` | 上下滚动 |
| `Space` | 向下翻页 |
| `b` | 向上翻页 |
| `/关键词` | 向下搜索 |
| `n` / `N` | 下一个 / 上一个搜索结果 |
| `q` | 退出 less |

例如：

```text
/error
```

搜索 error。

**解释**

- `less` = 分页查看文件内容
- 适合查看日志、大型文本文件
- 不会一次性加载全部内容，比 cat 更适合查看大文件

---

### 3.3 head（查看文件开头） <a id="cmd-head"></a>

**作用**

> 查看文件开头部分内容。

**用法**

```bash
# 查看文件前 10 行（默认）
head 文件名
```

例如：

```bash
head test.txt
```

```bash
# 指定查看行数
head -n 行数 文件名
```

例如：

```bash
# 查看前 20 行
head -n 20 test.txt
```

**解释**

- `head` = 查看头部
- 默认显示前 10 行
- `-n` = number，指定显示行数

---

### 3.4 tail（查看文件末尾） <a id="cmd-tail"></a>

**作用**

> 查看文件末尾部分内容。

**用法**

```bash
# 查看文件最后 10 行（默认）
tail 文件名
```

```bash
# 指定查看行数
tail -n 行数 文件名
```

例如：

```bash
# 查看最后 20 行
tail -n 20 test.txt
```

```bash
# 实时跟踪文件变化
tail -f 文件名
```

例如：

```bash
# 实时查看日志文件
tail -f app.log
```

按 `Ctrl + C` 退出跟踪。

**解释**

- `tail` = 查看尾部
- 默认显示最后 10 行
- `-n` = number，指定显示行数
- `-f` = follow，持续跟踪文件新增内容，常用于查看日志

---

### 3.5 wc（word count） <a id="cmd-wc"></a>

**作用**

> 统计文件中的行数、单词数和字节数。

**用法**

```bash
# 统计文件信息
wc 文件名
```

例如：

```bash
wc test.txt
```

输出：

```text
10 50 300 test.txt
```

表示：

```text
行数 单词数 字节数 文件名
```

```bash
# 只统计行数
wc -l 文件名
```

```bash
# 只统计单词数
wc -w 文件名
```

```bash
# 只统计字节数
wc -c 文件名
```

**解释**

- `wc` = word count
- `-l` = lines，行数
- `-w` = words，单词数
- `-c` = bytes，字节数（注意：中文等多字节字符占多个字节，不等于字符数）
- 常用于统计代码行数、日志数量等

---

### 3.6 grep（global regular expression print） <a id="cmd-grep"></a>

**作用**

> 在文件或命令输出中搜索指定内容，支持正则表达式。

**用法**

```bash
# 搜索文件中的内容
grep 关键词 文件名
```

例如：

```bash
# 查找 test.txt 中包含 hello 的行
grep hello test.txt
```

```bash
# 忽略大小写搜索
grep -i 关键词 文件名
```

```bash
# 显示匹配内容所在行号
grep -n 关键词 文件名
```

例如：

```bash
grep -n error log.txt
```

```bash
# 反向匹配：显示不包含关键词的行
grep -v 关键词 文件名
```

```bash
# 匹配行首（如查找以 # 开头的注释行）
grep "^# " 文件名
```

```bash
# 匹配行尾
grep "end$" 文件名
```

```bash
# 使用扩展正则，一次匹配多个关键词
grep -E "error|warning" log.txt
```

```bash
# 递归搜索目录中的文件
grep -r 关键词 目录
```

```bash
# 结合管道使用
命令 | grep 关键词
```

例如：

```bash
# 查看进程中是否存在 python
ps aux | grep python
```

**解释**

- `grep` = global regular expression print
- `-i` = ignore case，忽略大小写
- `-n` = number，显示行号
- `-v` = invert，反向匹配
- `-r` = recursive，递归搜索目录
- `-E` = 扩展正则表达式（`|` 表示"或"）
- `^` 匹配行首，`$` 匹配行尾
- 经常和 `|` 管道一起使用

---

### 3.7 nano（终端文本编辑器） <a id="cmd-nano"></a>

**作用**

> 使用终端编辑文本文件，简单易用。

**用法**

```bash
# 打开文件进行编辑
nano 文件名
```

例如：

```bash
# 编辑 hello.txt 文件
nano hello.txt
```

```bash
# 创建并编辑新文件（文件不存在时会自动创建）
nano test.txt
```

**nano 常用快捷键**

| 快捷键 | 作用 |
|---|---|
| `Ctrl + O` | 保存文件 |
| `Ctrl + X` | 退出 nano |
| `Ctrl + W` | 搜索内容 |
| `Ctrl + K` | 剪切当前行 |
| `Ctrl + U` | 粘贴剪切内容 |
| `Ctrl + G` | 打开帮助文档 |
| `Ctrl + C` | 显示当前光标位置 |

**解释**

- `nano` = 一个简单易用的终端文本编辑器
- 适合快速编辑配置文件
- nano 界面底部的快捷键提示中，`^` 表示 Ctrl

---

## 4. 权限与用户管理 <a id="permissions"></a>

### 4.1 chmod（change mode） <a id="cmd-chmod"></a>

**作用**

> 修改文件或目录的权限。

**用法**

```bash
# 修改文件权限
chmod 权限 文件名
```

例如：

```bash
# 给脚本添加执行权限（755 常用于脚本和程序）
chmod 755 script.sh
```

**Linux 文件权限**

查看权限：

```bash
ls -l
```

显示：

```text
-rwxr-xr-x 1 user user 1024 Aug  9 script.sh
```

权限分为三组：

| 权限位置 | 对象 |
|---|---|
| 第一组 `rwx` | 文件所有者（user） |
| 第二组 `r-x` | 所属用户组（group） |
| 第三组 `r-x` | 其他用户（other） |

**权限含义**

| 符号 | 含义 |
|---|---|
| `r` | read，读取权限 |
| `w` | write，写入权限 |
| `x` | execute，执行权限 |
| `-` | 没有权限 |

**数字方式设置权限**

权限对应数字：

| 权限 | 数字 |
|---|---|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |
| 无权限 | 0 |

常用组合：

| 权限 | 数字 |
|---|---|
| `rwx` | 7 |
| `rw-` | 6 |
| `r-x` | 5 |
| `r--` | 4 |

```bash
# 设置权限为 rwxr-xr-x
chmod 755 文件名
```

表示：

```text
所有者：rwx (7)
用户组：r-x (5)
其他用户：r-x (5)
```

```bash
# 设置权限为 rw-r--r--（普通文件常用）
chmod 644 文件名
```

表示：

```text
所有者：rw- (6)
用户组：r-- (4)
其他用户：r-- (4)
```

**字母方式设置权限**

```bash
# 给文件所有者增加执行权限
chmod u+x 文件名
```

```bash
# 删除文件所有者的执行权限
chmod u-x 文件名
```

```bash
# 给所有用户增加执行权限
chmod a+x 文件名
```

用户类型：

| 符号 | 含义 |
|---|---|
| `u` | user，文件所有者 |
| `g` | group，所属用户组 |
| `o` | other，其他用户 |
| `a` | all，所有用户 |

操作符：

| 符号 | 作用 |
|---|---|
| `+` | 增加权限 |
| `-` | 删除权限 |
| `=` | 直接设置权限 |

**目录权限**

```bash
chmod 755 目录名
```

可以设置目录权限。

> ⚠️ **注意**：目录的 `x` 权限表示"可以进入目录"。例如 `chmod 644 test` 会去掉目录的执行权限，导致目录可能无法正常访问。

**解释**

- `chmod` = change mode
- Linux 权限由所有者（user）、用户组（group）、其他用户（other）三部分组成
- 数字权限：r = 4，w = 2，x = 1
- 755 常用于脚本/程序，644 常用于普通文件

---

### 4.2 sudo（super user do） <a id="cmd-sudo"></a>

**作用**

> 以管理员（root）权限执行单条命令。

**用法**

```bash
# 使用管理员权限执行命令
sudo 命令
```

例如：

```bash
# 使用管理员权限在系统目录创建文件夹
sudo mkdir /opt/test
```

```bash
# 编辑系统配置文件
sudo nano /etc/hosts
```

**常用场景**

- 安装 / 卸载软件（`sudo apt install ...`）
- 修改系统配置文件
- 管理用户和权限

**解释**

- `sudo` = super user do
- 用于临时获得管理员权限，只对当前这一条命令生效（短时间内有缓存）
- 普通用户默认没有修改系统文件的权限
- 与 `su` 的区别：`su` 是切换到 root 用户，`sudo` 是借 root 权限执行单条命令

> ⚠️ **注意**：sudo 权限很高，执行删除、修改系统文件等操作前请确认命令无误。

---

### 4.3 useradd（user add） <a id="cmd-useradd"></a>

**作用**

> 创建新用户并设置登录密码。

**用法**

```bash
# 创建用户
sudo useradd 用户名
```

例如：

```bash
# 创建用户 test
sudo useradd test
```

```bash
# 创建用户并明确创建主目录
sudo useradd -m 用户名
```

```bash
# 创建用户并指定登录 Shell
sudo useradd -s /bin/bash 用户名
```

**解释**

- `useradd` = user add
- `-m` = 创建用户主目录。Ubuntu / Debian 上 useradd 默认就会创建主目录（/etc/login.defs 中 CREATE_HOME=yes），在其他发行版上需要显式加 `-m`
- `-s` = shell，指定登录 Shell
- 创建用户通常需要 sudo 权限
- 用户主目录默认位于 `/home/用户名`

---

### 4.4 userdel（user delete） <a id="cmd-userdel"></a>

**作用**

> 删除用户。

**用法**

```bash
# 删除用户
sudo userdel 用户名
```

例如：

```bash
# 删除 test 用户
sudo userdel test
```

```bash
# 删除用户并同时删除主目录
sudo userdel -r 用户名
```

**解释**

- `userdel` = user delete
- `-r` = remove home directory，同时删除用户主目录
- 删除用户需要 sudo 权限

---

### 4.5 用户组管理（groupadd / groupdel / usermod） <a id="cmd-group"></a>

**作用**

> 管理用户组，用于对多个用户进行统一的权限管理。

**用法**

```bash
# 查看所有用户组
cat /etc/group
```

```bash
# 创建用户组
sudo groupadd 组名
```

```bash
# 删除用户组
sudo groupdel 组名
```

```bash
# 修改用户组名称
sudo groupmod -n 新组名 原组名
```

```bash
# 查看用户所属的组
groups 用户名
```

```bash
# 将用户添加到用户组
sudo usermod -aG 组名 用户名
```

例如：

```bash
# 创建 dev 组，并把 test 用户加进去
sudo groupadd dev
sudo usermod -aG dev test
```

**解释**

- `groupadd` = 创建用户组
- `groupdel` = 删除用户组
- `groupmod` = 修改用户组信息
- `groups` = 查看用户所属组
- `usermod -aG` = 将用户追加到指定组（`-a` = append，不加 `-a` 会覆盖用户原有的附加组）

Linux 权限管理主要由三部分组成：

| 对象 | 含义 |
|---|---|
| user | 文件所有者 |
| group | 文件所属用户组 |
| other | 其他用户 |

---

## 5. 系统资源与进程 <a id="system"></a>

### 5.1 top（实时进程监控） <a id="cmd-top"></a>

**作用**

> 实时查看系统中的进程状态和资源占用情况。

**用法**

```bash
# 查看系统进程
top
```

**修改刷新频率**

默认每 3 秒刷新一次。按 `s` 后输入秒数即可修改：

```text
s
1
```

表示每 1 秒刷新一次。

**top 常用操作**

| 按键 | 作用 |
|---|---|
| `q` | 退出 top |
| `P` | 按 CPU 使用率排序 |
| `M` | 按内存使用率排序 |
| `s` | 修改刷新间隔 |
| `k` | 输入 PID 后结束进程 |
| `1` | 显示每个 CPU 核心状态 |
| `h` | 查看帮助 |

**解释**

- `top` 并没有官方全称，"table of processes" 是常见的流传说法
- `PID` = Process ID，进程编号
- `%CPU` = CPU 使用率
- `%MEM` = 内存使用率
- 动态实时监控用 top，一次性查看快照用 ps

---

### 5.2 ps（process status） <a id="cmd-ps"></a>

**作用**

> 查看进程信息。与 top 的动态监控不同，ps 是一次性快照。

**用法**

```bash
# 查看当前终端启动的进程
ps
```

```bash
# 查看系统中所有进程（最常用）
ps aux
```

```bash
# 查找指定进程
ps aux | grep python
```

**解释**

- `ps` = process status
- `a` = 显示所有用户的进程
- `u` = 以用户友好的格式显示（含 CPU / 内存占用）
- `x` = 包含没有终端的后台进程
- 常用列：
  - `PID` = 进程编号（配合 kill 使用）
  - `%CPU` / `%MEM` = CPU / 内存占用率
  - `STAT` = 进程状态（S 睡眠、R 运行、Z 僵尸等）
  - `COMMAND` = 启动进程的命令
- 另一种写法 `ps -ef` 效果类似

---

### 5.3 kill（向进程发送信号） <a id="cmd-kill"></a>

**作用**

> 向进程发送信号，通常用于结束进程。

**用法**

```bash
# 根据 PID 结束进程（默认发送 SIGTERM，给进程保存数据的机会）
kill PID
```

例如：

```bash
# 结束 PID 为 1234 的进程
kill 1234
```

```bash
# 强制结束进程（SIGKILL，进程无法忽略）
kill -9 PID
```

```bash
# 按名称结束进程
pkill 进程名
```

例如：

```bash
# 结束所有名为 python 的进程
pkill python
```

**查看进程 PID**

```bash
# 动态查看（按 q 退出）
top
```

或者：

```bash
# 静态查看
ps aux
```

**解释**

- `kill` = 发送信号给进程
- `PID` = Process ID，进程编号
- `-15`（默认）= SIGTERM，温和终止，进程可以保存数据后退出
- `-9` = SIGKILL，强制终止，进程无法处理
- 建议先尝试默认的 `kill PID`，无效再用 `-9`

> ⚠️ **注意**：`kill -9` 会立即结束进程，可能导致数据未保存。

---

### 5.4 free（内存使用情况） <a id="cmd-free"></a>

**作用**

> 查看系统内存和交换空间使用情况。

**用法**

```bash
# 查看内存使用情况
free
```

```bash
# 以易读方式显示
free -h
```

输出示例：

```text
               total        used        free      shared  buff/cache   available
Mem:            16G        8.1G        2.3G        120M        5.6G        7.2G
Swap:          2.0G          0B        2.0G
```

**解释**

- `free` = 查看内存使用情况
- `-h` = human readable，以易读单位显示
- `Mem` = 物理内存
- `Swap` = 交换空间（磁盘上划出的虚拟内存）
- `available` = 真正可用的内存（包含可回收的缓存），比 `free` 更值得关注

---

### 5.5 df（disk free） <a id="cmd-df"></a>

**作用**

> 查看磁盘空间使用情况。

**用法**

```bash
# 查看磁盘空间
df
```

```bash
# 以易读方式显示
df -h
```

输出示例：

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       100G   50G   50G  50% /
```

**解释**

- `df` = disk free
- `-h` = human readable
- 查看文件系统（磁盘分区）的剩余空间，常用于检查磁盘是否不足
- 查看某个目录占用多少空间用 du

---

### 5.6 du（disk usage） <a id="cmd-du"></a>

**作用**

> 查看文件或目录占用的磁盘空间。

**用法**

```bash
# 查看目录大小
du 目录名
```

```bash
# 以易读方式显示大小
du -h 目录名
```

```bash
# 查看目录总大小
du -sh 目录名
```

例如：

```bash
du -sh project
```

输出：

```text
500M    project
```

```bash
# 查看当前目录下各子目录的大小
du -h --max-depth=1
```

**解释**

- `du` = disk usage
- `-h` = human readable，以易读单位显示
- `-s` = summarize，只显示总大小
- `--max-depth=N` = 只统计到第 N 层子目录
- df 看磁盘整体剩余空间，du 看具体目录占用

---

## 6. 软件管理 <a id="packages"></a>

### 6.1 apt（Advanced Package Tool） <a id="cmd-apt"></a>

**作用**

> 管理 Ubuntu 系统中的软件包，包括安装、更新、卸载软件。

**用法**

```bash
# 更新软件源列表
sudo apt update
```

```bash
# 升级已安装的软件
sudo apt upgrade
```

```bash
# 安装软件
sudo apt install 软件名
```

例如：

```bash
# 安装 git
sudo apt install git
```

```bash
# 卸载软件（保留配置文件）
sudo apt remove 软件名
```

```bash
# 完全卸载软件（包括配置文件）
sudo apt purge 软件名
```

```bash
# 清理不再需要的依赖包
sudo apt autoremove
```

```bash
# 搜索软件
apt search 软件名
```

例如：

```bash
# 搜索 python 相关软件包
apt search python
```

```bash
# 查看已安装的软件
apt list --installed
```

**解释**

- `apt` = Advanced Package Tool
- 用于管理 Debian / Ubuntu 系统的软件包
- `apt update` = 更新软件源信息（只更新列表，不安装）
- `apt upgrade` = 升级已安装的软件
- `apt install` = 安装软件
- `apt remove` = 卸载软件，保留配置文件
- `apt purge` = 完全卸载，连配置文件一起删除
- `apt autoremove` = 清理不再需要的依赖
- 安装、卸载软件通常需要 sudo 权限

---

## 7. 网络 <a id="network"></a>

### 7.1 ifconfig 与 ip（网络接口） <a id="cmd-ifconfig"></a>

**作用**

> 查看和配置网络接口。ifconfig 已过时，新版 Ubuntu 推荐使用 ip 命令。

**用法**

```bash
# 查看所有网络接口及 IP（新版推荐）
ip addr
```

```bash
# 查看指定网络接口
ip addr show 接口名
```

```bash
# 查看接口启用状态
ip link show
```

旧命令（未安装时需 `sudo apt install net-tools`，不建议新学）：

```bash
# 查看网络接口信息（旧）
ifconfig
```

```bash
# 启用 / 关闭网络接口
sudo ifconfig 接口名 up
sudo ifconfig 接口名 down
```

**解释**

- `ip addr` 等价于旧命令 `ifconfig`
- `ifconfig` = interface configuration
- 常见网络接口：
  - `lo`：本地回环接口（127.0.0.1）
  - `enp3s0` / `eth0`：有线网络（新版使用可预测命名，如 enp3s0）
  - `wlp2s0` / `wlan0`：无线网络
- 用 `ip addr` 查看本机实际接口名，再操作对应接口

---

### 7.2 netstat 与 ss（网络连接） <a id="cmd-netstat"></a>

**作用**

> 查看网络连接、监听端口和对应进程。netstat 已过时，新版推荐使用 ss。

**用法**

```bash
# 查看监听端口及对应进程（新版推荐）
ss -tulnp
```

输出示例：

```text
State  Recv-Q Send-Q Local Address:Port  Peer Address:Port Process
LISTEN 0      128    0.0.0.0:22          0.0.0.0:*        users:(("sshd",pid=1234,fd=3))
```

旧命令（未安装时需 `sudo apt install net-tools`）：

```bash
# 查看监听端口及对应进程（旧）
netstat -tulnp
```

**参数说明**

| 参数 | 作用 |
|---|---|
| `-t` | 查看 TCP 连接 |
| `-u` | 查看 UDP 连接 |
| `-l` | 仅显示监听状态 |
| `-n` | 直接显示 IP 和端口，不做名称解析 |
| `-p` | 显示对应进程 |

**解释**

- `netstat` = network statistics
- 常用于：
  - 检查服务是否启动
  - 查看开放端口
  - 排查网络问题

---

## 8. 压缩与归档 <a id="archive"></a>

### 8.1 tar（tape archive） <a id="cmd-tar"></a>

**作用**

> 打包和解压文件，是 Linux 中最常用的归档工具。

**用法**

```bash
# 打包文件（只打包，不压缩）
tar -cvf 打包名.tar 文件或目录
```

例如：

```bash
# 将 project 文件夹打包成 project.tar
tar -cvf project.tar project
```

```bash
# 解包文件
tar -xvf 打包名.tar
```

```bash
# 打包并使用 gzip 压缩
tar -czvf 压缩包名.tar.gz 文件或目录
```

例如：

```bash
# 将 project 压缩为 project.tar.gz
tar -czvf project.tar.gz project
```

```bash
# 解压 tar.gz 文件
tar -xzvf 压缩包名.tar.gz
```

```bash
# 查看压缩包内容（不解压）
tar -tvf 压缩包名.tar.gz
```

**tar 常用参数**

| 参数 | 作用 |
|---|---|
| `-c` | create，创建压缩包 |
| `-x` | extract，解压文件 |
| `-v` | verbose，显示过程 |
| `-f` | file，指定文件名 |
| `-z` | gzip，使用 gzip 压缩 |
| `-t` | list，查看内容（不解压） |

**解释**

- `tar` = tape archive
- `.tar` = 只打包，不压缩
- `.tar.gz` = 打包并使用 gzip 压缩
- 解压 tar.gz 也可以用 `tar -xf`（新版 tar 会自动识别压缩格式）

---

## 9. Shell 基础 <a id="shell"></a>

### 9.1 echo（输出内容） <a id="cmd-echo"></a>

**作用**

> 输出指定内容，也可以查看环境变量。

**用法**

```bash
# 输出文本
echo "Hello Linux"
```

输出：

```text
Hello Linux
```

```bash
# 输出变量内容
echo "$变量名"
```

例如：

```bash
# 查看 PATH 环境变量
echo "$PATH"
```

```bash
# 输出后不换行
echo -n "Hello"
```

```bash
# 解析转义字符（\n 换行、\t 制表符）
echo -e "第一行\n第二行"
```

```bash
# 将内容写入文件（覆盖）
echo "Hello" > test.txt
```

```bash
# 追加内容到文件
echo "Linux" >> test.txt
```

**解释**

- `echo` = 回显、输出
- `$` 用于引用变量
- 内容含有空格或变量时，建议使用双引号包起来
- `-n` = 输出后不换行
- `-e` = 解析转义字符
- `>` = 覆盖写入文件（会先清空原文件）
- `>>` = 追加写入文件（保留原内容）

> ⚠️ **注意**：`>` 会清空原文件内容后写入，误用会覆盖重要文件。

---

### 9.2 .bashrc（bash run commands） <a id="cmd-bashrc"></a>

**作用**

> Bash 的配置文件，用于设置环境变量、别名和启动时自动执行的命令，每次打开终端都会加载。

**用法**

```bash
# 查看 .bashrc 文件
cat ~/.bashrc
```

```bash
# 编辑 .bashrc 文件
nano ~/.bashrc
```

```bash
# 添加自定义别名，例如：
alias update='sudo apt update'
```

保存后重新加载，立即生效：

```bash
source ~/.bashrc
```

**解释**

- `.bashrc` = bash run commands，位于用户主目录（`~`）
- 常用于：
  - 设置环境变量
  - 创建命令别名
  - 自定义提示符
- 修改后需要 `source ~/.bashrc` 或重新打开终端才能生效
- Ubuntu 的默认 .bashrc 已包含 `ll`、`la` 等别名和彩色提示符
- 区分：登录时执行 `~/.profile`，交互终端执行 `~/.bashrc`（Ubuntu 的 .profile 会自动加载 .bashrc）

---

### 9.3 Shell 脚本（.sh） <a id="cmd-shell-script"></a>

**作用**

> 把一系列命令写入文件，实现批量、可重复执行。

**用法**

```bash
# 创建脚本文件
touch script.sh
```

```bash
# 编辑脚本
nano script.sh
```

内容例如：

```bash
#!/bin/bash

echo "Hello Linux"
date
```

```bash
# 添加执行权限
chmod +x script.sh
```

```bash
# 执行脚本
./script.sh
```

输出：

```text
Hello Linux
Wed Oct  8 18:58:00 CST 2026
```

```bash
# 或直接用 bash 执行（不需要执行权限）
bash script.sh
```

**解释**

- `.sh` = Shell Script 文件扩展名
- `#!/bin/bash` = shebang，指定使用 Bash 解释器执行
- `chmod +x` = 添加执行权限
- `./` 表示当前目录（Linux 默认不在当前目录查找命令，所以需要写 `./`）

---

### 9.4 同时执行多个命令（; && || |） <a id="cmd-multi"></a>

**作用**

> 在一行中组合执行多个命令。

**用法**

**`;` 依次执行**

```bash
命令1 ; 命令2
```

无论前一个命令是否执行成功，都会执行后面的命令。

```bash
ls ; pwd
```

输出示例：

```text
Desktop  Documents  test.txt
/home/user
```

**`&&` 前一个成功后执行**

```bash
命令1 && 命令2
```

只有前一个命令执行成功，才会执行后面的命令。

```bash
mkdir test && cd test
```

创建目录成功，才进入 test。

**`||` 前一个失败后执行**

```bash
命令1 || 命令2
```

只有前一个命令执行失败，才会执行后面的命令。

```bash
mkdir test || echo "创建失败"
```

创建失败时输出"创建失败"。

**`|` 管道**

```bash
命令1 | 命令2
```

将前一个命令的输出作为后一个命令的输入。

```bash
# 查找当前目录中包含 txt 的文件
ls | grep txt
```

```bash
# 统计文件行数
cat test.txt | wc -l
```

**总结**

| 符号 | 作用 |
|---|---|
| `;` | 依次执行，不关心成功失败 |
| `&&` | 前一个成功后执行下一个 |
| `||` | 前一个失败后执行下一个 |
| `|` | 管道，将输出传给下一个命令 |

---

## 10. 系统信息与帮助 <a id="info"></a>

### 10.1 date（查看与设置时间） <a id="cmd-date"></a>

**作用**

> 查看和设置系统日期时间。

**用法**

```bash
# 查看当前时间
date
```

输出：

```text
Wed Oct  8 18:58:00 CST 2026
```

```bash
# 自定义时间显示格式
date +"格式"
```

例如：

```bash
# 显示年月日
date +"%Y-%m-%d"
```

输出：

```text
2026-10-08
```

```bash
# 手动设置系统时间
sudo date -s "2026-10-08 12:00:00"
```

```bash
# 新版推荐使用 timedatectl 查看 / 设置时间
timedatectl
sudo timedatectl set-time "2026-10-08 12:00:00"
```

**常用格式**

| 格式 | 含义 |
|---|---|
| `%Y` | 年份 |
| `%m` | 月份 |
| `%d` | 日期 |
| `%H` | 小时 |
| `%M` | 分钟 |
| `%S` | 秒 |

**解释**

- `date` = 日期
- 常用于查看系统时间和在脚本中生成时间信息

---

### 10.2 cal（calendar） <a id="cmd-cal"></a>

**作用**

> 查看日历。

**用法**

```bash
# 查看当前月份日历
cal
```

```bash
# 查看指定年份日历
cal 年份
```

例如：

```bash
# 查看 2026 年日历
cal 2026
```

```bash
# 查看指定月份日历
cal 月份 年份
```

例如：

```bash
# 查看 2026 年 8 月日历
cal 8 2026
```

**解释**

- `cal` = calendar
- 用于快速查看日期

---

### 10.3 which（查找命令位置） <a id="cmd-which"></a>

**作用**

> 查找命令对应的可执行文件位置。

**用法**

```bash
# 查找命令路径
which 命令
```

例如：

```bash
# 查找 ls 命令的位置
which ls
```

输出：

```text
/usr/bin/ls
```

```bash
# 查找 python3 位置（新版 Ubuntu 默认只安装 python3）
which python3
```

**解释**

- `which` = 查找命令位置
- 用于确认当前执行的是哪个程序
- Linux 中很多命令实际上对应某个可执行文件
- 新版 Ubuntu 中 `which python` 可能没有结果，因为默认只有 python3

---

### 10.4 whatis（命令简介） <a id="cmd-whatis"></a>

**作用**

> 查看命令的简单说明。

**用法**

```bash
# 查看命令简介
whatis 命令
```

例如：

```bash
# 查看 ls 的说明
whatis ls
```

输出：

```text
ls (1) - list directory contents
```

```bash
# 查看多个命令说明
whatis ls cd mkdir
```

**解释**

- `whatis` = 查看是什么
- 用于快速了解命令用途
- 类似命令的简短帮助说明
- 更详细的文档用 man

> 注意：如果 `whatis` 没有结果，可以先重建手册索引：`sudo mandb`。

---

### 10.5 man（manual） <a id="cmd-man"></a>

**作用**

> 查看 Linux 命令的详细帮助文档。

**用法**

```bash
# 查看命令帮助
man 命令
```

例如：

```bash
# 查看 ls 命令的帮助
man ls
```

进入帮助页面：

```text
LS(1)              User Commands              LS(1)

NAME
       ls - list directory contents
```

**man 中常用操作**

| 按键 | 作用 |
|---|---|
| `↑` / `↓` | 上下滚动查看内容 |
| `Space` | 向下翻页 |
| `b` | 向上翻页 |
| `/关键词` | 搜索内容 |
| `n` / `N` | 下一个 / 上一个搜索结果 |
| `q` | 退出 man |

**查看命令所在章节**

```bash
man -f 命令
```

例如：

```bash
man -f printf
```

显示：

```text
printf (1) - format and print data
printf (3) - formatted output conversion
```

**查看指定章节**

```bash
man 章节 命令
```

例如：

```bash
man 1 printf
```

表示查看用户命令中的 printf。

**Linux man 章节**

| 章节 | 内容 |
|---|---|
| 1 | 用户命令 |
| 2 | 系统调用 |
| 3 | 库函数 |
| 4 | 设备文件 |
| 5 | 配置文件格式 |
| 6 | 游戏 |
| 7 | 杂项 |
| 8 | 系统管理员命令 |

**解释**

- `man` = manual，手册
- 用于查看 Linux 命令的官方帮助文档
- 比 `whatis` 提供的信息更加详细
- 学习 Linux 时遇到不会的命令，可以优先使用：

```bash
man 命令
```

---

### 10.6 clear（清空终端） <a id="cmd-clear"></a>

**作用**

> 清空终端屏幕。

**用法**

```bash
clear
```

快捷键 `Ctrl + L` 效果相同。
