# 学习日志

> 从零做一个博客的进度与笔记。每完成一个阶段，追加一节。
> 项目根目录：`D:\my-blog`

---

## 阶段 0 · 地基与心智模型 — ✅ 完成（2026-11-25 星期三）

### 0.1 一次访问的完整链路（8 步）

输入 `https://blog.example.com/post/1` 回车后：

1. **查缓存** —— 浏览器缓存 → 系统 `hosts` 文件（可手动把域名写死成 IP，DNS 出问题时的排查手段）
2. **DNS 解析** —— 域名翻译成 IP（分布式查表服务）
3. **建连接** —— TCP 三次握手；`https` 再加一次 TLS 握手协商加密
4. **发 HTTP 请求** —— 方法（GET）+ 路径 + 一堆请求头
5. **服务器响应** —— 状态码（200 成功 / 404 没有 / 500 服务器炸了）+ 响应头 + body
6. **解析成 DOM** —— 浏览器把 HTML 解析成一棵树
7. **渲染** —— CSS 规则 + DOM → 算布局（谁在哪、多宽多高）→ 绘制像素
8. **屏幕出现内容**

### 0.2 IP / 端口 / 域名

| 概念 | 解决什么问题 | 比喻 |
|---|---|---|
| IP | 找到**哪台机器** | 门牌号 |
| 端口 | 找到机器上的**哪个程序** | 房间号 |
| 域名 | 让人**记得住** | 名字，靠 DNS 翻译成 IP |

- `localhost:4321` = 本机（`127.0.0.1`）的 4321 端口，本地开发服务器住这儿
- **DNS 方向是「域名 → IP」**（正向解析；反向也能查，叫 PTR，日常不用）
- DNS 结果**有缓存（TTL）**，所以改了域名解析不会立刻全球生效

### 0.3 静态站 / 动态站 / SSG / SSR / CSR —— 全路线最核心的概念

**一句话类比：**

```
.md 源码  →  构建  →  dist/*.html     等于     .c 源码  →  gcc  →  .exe
         构建 = 编译；SSG 就是「编译期把所有页面算好」
```

| 类型 | 计算时机 | 类比 | 形态 |
|---|---|---|---|
| 动态站 | 每次请求 | 运行期计算 | 每次访客来都跑代码、查数据库、拼 HTML |
| **SSG**（静态站点生成） | **构建时一次** | **编译期计算** | 构建出 50 个 `.html`，服务器只发文件 |
| SSR | 每次请求（服务器端） | 运行期计算 | 服务器渲染出 HTML 再发 |
| CSR | 浏览器里 | 客户端运行期计算 | 先给空壳 + JS，浏览器自己拼 |

**博客选 SSG 的理由：**
- 内容变化频率低，没必要每个访客都算一遍
- 没有登录、没有数据库 → 没有后端 → 没有攻击面
- 页面就是文件，丢 CDN 上全球都快
- 托管免费，只需域名钱（甚至域名也不买）

**代价：**
- 内容变了必须**重新构建**
- 做不了「用户能提交数据」的功能（这就是阶段 10 若要做动态站得换 Next.js 的原因）

### 0.4 路径与扩展名

- **绝对路径**：`D:\my-blog\index.html` —— 从盘符开始的完整地址
- **相对路径**：`./posts/a.html`（当前目录）、`../index.html`（**`..` = 上一级**）
- **扩展名决定谁来解释这个文件**：`.html` 给浏览器、`.md` 给 Markdown 渲染器、`.c` 给编译器
- **项目命名规范**：不要空格、中文、单引号、括号（很多命令行工具在这些字符上会挂。本机 `C:\Users\he'ro` 就是现成的反面教材）

### 0.5 PATH

**定义：** 一个环境变量，内容是一串**目录**，分号分隔。敲命令时系统**按顺序**在这些目录里找同名 `.exe`，**第一个匹配的获胜（顺序 = 优先级）**。

- 类比：C 的 **include 搜索路径**（`-I` 列表）。找不到的报错是同一性质的
- **两种「命令找不到」**：① 目录没进 PATH ② 目录在 PATH 里但里面没这个文件
- 查看全部条目：`$env:PATH -split ';'`
- 查某命令从哪来：`Get-Command node` → 看 `Source` 列

### 0.6 DOM 与 CSS（一句话版本）

```
HTML 文本 → 解析 → DOM 树 → 加 CSS 规则 → 算布局 → 画像素 → 屏幕出现
```

| 浏览器世界 | 编译原理世界 |
|---|---|
| `.html` 源文件 | 源文件 |
| **DOM**（内存里的树） | **解析后的语法树 / AST** |
| CSS 规则 | 属性/约束表 |
| 渲染出画面 | 遍历求值、输出 |

- **DOM 不是文件**，是浏览器读完 HTML 后在内存里建出来的数据结构。每个标签一个节点，嵌套即父子，`<html>` 是根
- `F12` 元素面板看到的 = 你源文件的**解析结果**
- **HTML 管「有什么」，CSS 管「长什么样」**

### 0.7 本机环境实测

| 项目 | 结果 |
|---|---|
| 终端 | Windows PowerShell 5.1（够用） |
| Node | `v24.21.0`，位置 `D:\node.exe`（装在 D 盘根，不太规范但能用，**别乱动**） |
| npm | `11.19.0` |
| Git | `2.55.0.windows.5`，来自 `D:\Git\cmd` |
| VS Code | `code` 命令可用，来自 `D:\Microsoft VS Code\bin` |
| 项目根目录 | `D:\my-blog` |

**PATH 里读出的信息：**
- `C:\WINDOWS\system32` 排最前 → 系统命令永远优先
- `D:\` 这一行 → `node` 能被找到的原因（但它也意味着 D 盘根目录**任何 exe** 都能直接敲，不够干净）
- `D:\MinGW\mingw64\bin` → gcc 在这儿

### 0.8 自测题与批改

| 题 | 我的回答 | 结果 |
|---|---|---|
| 1. 访问链路 | 查缓存 → DNS → TCP → 发请求 → 服务器响应 | ✅ 前 5 步全对；补上了 DOM 解析与渲染 |
| 2. IP / 端口 / 域名 | IP 是地址、端口是大门、域名是方便记忆的名字 | ⚠️ 基本对，但 **DNS 方向说反了**（是 域名→IP，不是 IP→域名） |
| 3. SSG vs 动态站 | 一开始不懂 | ✅ 换成「编译期 / 运行期」类比后讲通，复述正确 |
| 4. PATH | 一开始不懂 | ✅ 换成「include 路径」类比后讲通，复述正确 |

### 0.9 阶段 0 结论

**已能讲清：** 请求链路、IP/端口/域名分工、SSG = 编译期计算、PATH 的搜索机制、DOM 是解析后的树。

**进入阶段 1 时要注意的：**
- HTML 只描述结构和含义，**不描述长相**
- 用语义化标签（`header`/`nav`/`main`/`article`/`footer`），不要全用 `div`
- `file://` 协议 = 浏览器直接从硬盘读文件，没有服务器参与
- 中文乱码 = 编码问题（`<meta charset="UTF-8">`）

### 0.10 本阶段还没搞懂的

- （想不起来就先留空，想起来了补上）

---

## 阶段 1 · HTML：给页面立骨架 — ✅ 完成（2026-11-26）

**产出**
- `D:\my-blog\index.html` —— 首页：header / nav / main（3 个 article）/ footer + 图片
- `D:\my-blog\posts\hello.html` —— 第一篇文章：h1 / time / 3 段 p / h2 / ul / blockquote / pre+code / 返回链接
- 首页 ↔ 文章页**双向跳转**通了

### 1.1 标签、元素、属性

- `<a href="x">文字</a>` = 开始标签 + 内容 + 结束标签 = **一个元素**
- `href` 是**属性**，键值对，只能写在开始标签里
- 类比：一个元素 ≈ 一个 struct,属性 ≈ 它的字段
- `img` 这类没有内容和结束标签：`<img src="..." alt="...">`

### 1.2 文档骨架每一行的作用

| 行 | 作用 |
|---|---|
| `<!DOCTYPE html>` | 标准模式。不写会走老 IE 的「怪异模式」，布局会错位 |
| `lang="zh-CN"` | 声明语言，影响读屏发音和翻译提示 |
| `<meta charset="UTF-8">` | 编码。不写/写错 → 中文乱码 |
| `<meta name="viewport">` | 手机端宽度策略，阶段 2 响应式必写 |
| `<title>` | 标签页文字 + 搜索引擎结果第一行 |
| `head` / `body` | 关于页面的信息 / 用户看得见的内容 |

### 1.3 语义化标签

`header` `nav` `main` `article`（一篇文章就用它）`section` `aside` `footer`

- 默认长相和 `div` 几乎一样，**不会自动变好看**，视觉归 CSS 管
- 价值：搜索引擎和读屏软件靠它理解结构；**命名即文档**

### 1.4 两个必须转义的字符

| 想显示 | 写成 |
|---|---|
| `<` | `&lt;` |
| `>` | `&gt;` |
| `&` | `&amp;` |

- `<pre>` **保留所有空格和换行**，所以 pre 里的内容要**顶格写**，否则 HTML 缩进会原样出现在页面上
- `<pre><code>` 组合：`pre` 管格式，`code` 标记这是代码

### 1.5 `<time datetime="...">` 的两套值

- 标签中间的文字：**给人看**（2026 年 11 月 26 日）
- `datetime` 属性：**给机器看**，必须是 `YYYY-MM-DD`
- 踩过的坑：datetime 写 2025、文字写 2026，机器读到的是错的日期

### 1.6 踩过的坑：文件 ≠ 文件夹

现象：建成了 `posts\hello.html\hello.html` —— 一个**叫 hello.html 的文件夹**，文件在里面。

后果：
- 首页链 `posts/hello.html` 撞上的是**文件夹**，打不开
- 文章页 `../index.html` 从 `posts\hello.html\` 往上跳一级 → 找 `posts\index.html` → 不存在，回不去

修法（顺序不能反）：

```powershell
cd D:\my-blog\posts
Rename-Item .\hello.html _bad
Move-Item .\_bad\hello.html .\hello.html
Remove-Item .\_bad
```

教训：
- VS Code 的「新建文件」和「新建文件夹」是**两个不同的图标**
- Windows 允许文件和文件夹**同名**，浏览器只看路径，分不清你要哪个

### 1.7 报错怎么读

```
找不到路径"D:\my-blog\posts\bad"      ← 实际少敲了一个下划线
```

**报错信息会把它「理解的」路径原样打出来。** 拿它跟你心里的预期比对，差在哪一眼可见。这是排查的第一动作。

### 1.8 源文件 vs DOM（本阶段最重要的概念）

| 源文件 | DOM |
|---|---|
| 磁盘上的**文本** | 内存里的**对象树** |
| 给**人**写的 | 给**程序**读写用的 |
| 持久存在 | 页面打开期间存在，刷新就重建 |
| 相当于 `.c` 源文件 | 相当于加载到内存里的 AST / 数据结构 |
| 编辑它 | 相当于在调试器里改变量 |

三条本质区别：

1. **形态不同** —— 文件是字节文本；DOM 每个节点是对象，有属性和方法（JS 能操作）
2. **内容可能不同** —— 浏览器会补全和规范化：自动补 `tbody`、标签统一小写、错误嵌套自动纠正
3. **生命周期不同** —— **改 DOM ≠ 改文件**，刷新就还原

亲手验证过：
- F12 改文字 → 页面立刻变 → F5 刷新还原 → 文件一个字节没动
- `Ctrl+U`「查看网页源代码」看的是**文件**，F12「元素」看的是**DOM 树**
- Console 里输入 `document.title` 能拿到标题 → 证明 DOM 是能被程序访问的对象

**一句话：文件是「源码」，DOM 是「加载后的内存结构」。F12 是调试器，不是编辑器。**

### 1.9 本阶段还没搞懂的

- （待补）

---

## 阶段 2 · CSS：让它长得像博客 — ✅ 完成（2026-11-26）

**产出**：`D:\my-blog\style.css` + 两个页面通过 `<link>` 接入；首页从「满屏巨字」变成了一栏居中的卡片式布局，手机端正常。

### 2.1 CSS 放哪里

- 行内 `style=""`：和结构混在一起，改一处要改一堆 → ❌
- 内部 `<style>`：只能管当前页面 → ⚠️
- **外部 `<link rel="stylesheet" href="style.css">`：一处改全站变 → ✅ 只用这个**

相对路径注意：`index.html` 用 `style.css`，`posts/hello.html` 用 `../style.css`（先跳回上一层）。

### 2.2 选择器与优先级

| 写法 | 选中什么 |
|---|---|
| `h1` | 所有 h1 元素 |
| `.card` | `class="card"` 的元素（最常用） |
| `#main` | id 唯一元素（少用） |
| `article h2` | `article` 里面的 `h2`（空格 = 层级，**后代选择器**） |
| `a:hover` | 悬停状态（**伪类**） |
| `*` | 所有元素（**通配符**） |

优先级：`#id` > `.类` > `元素`；同级**后写的赢**。`!important` 不要用。

### 2.3 盒模型（重点）

四层由内到外：`content` → `padding`（内边距，背景色铺满）→ `border`（边框）→ `margin`（外边距，永远透明）

- 默认 `width` **只算 content**，加 padding 会把盒子撑大
- 所以第一行永远是：`*, *::before, *::after { box-sizing: border-box; }`
- `margin: 0 0 8px` 简写顺序 = **上 右 下 左**（顺时针）
- `margin: 0 auto` = 左右均分剩余空间 = **水平居中**

### 2.4 单位与排版

| 单位 | 含义 | 用途 |
|---|---|---|
| `px` | 绝对像素 | 边框、小间距 |
| `rem` | 相对根字号（1rem = 16px） | **字号首选**，用户调字号能跟着缩放 |
| `%` | 相对父容器 | 宽度 |
| `vw` | 视口宽度 1% | 满屏区块 |

- `line-height: 1.7` **不带单位** = 字号的 1.7 倍；写 `1.7em` 会在继承时算错
- `font-family` 是**降级链**，像 C 的多级 fallback

### 2.5 Flexbox

给容器 `display: flex`，子元素沿**主轴**（默认水平）排成一行：

| 属性 | 作用 |
|---|---|
| `gap` | 子元素之间的间距，**只有 flex/grid 容器能用** |
| `justify-content` | 主轴（水平）排列：`flex-start` / `center` / `space-between` / `flex-end` |
| `align-items` | 交叉轴（垂直）对齐：`center` 常用 |

**`gap` 和 `margin` 的区别**：`margin` 是元素自己的外间距，会作用在它与**所有**邻居之间；`gap` 是**容器**决定的"内部网格间距"，只作用于容器的子元素之间，不会给最外侧加多余空隙。

### 2.6 响应式

```css
@media (max-width: 600px) {
  body { font-size: 15px; }
}
```

读作「**视口宽度 ≤ 600px 时**才生效」。前提是 HTML 里有 `<meta name="viewport" content="width=device-width, initial-scale=1.0">`，没有它手机假装自己是 980px 宽，媒体查询永不触发。

验收方式：`F12` → `Ctrl+Shift+M` **设备模拟**（职业前端每天在用）。

### 2.7 图片处理

```css
header img {
  display: block;
  width: 120px;
  height: 120px;
  object-fit: cover;      /* 裁剪填充，不变形 */
  border-radius: 50%;     /* 正圆 */
}
```

- `object-fit: cover` 是图片变形的克星
- `img` 默认 inline，底部会有几像素空隙 → `display: block` 治它
- `max-width: 100%` 防止图片溢出容器

### 2.8 踩过的坑：CSS 拼错不报错

写成 `*::bedore`（before 拼错）→ **整条规则被丢弃**，不是跳过那一个选择器。

- CSS 的规矩：逗号分隔的选择器列表里**只要有一个无效，整条规则作废**
- 后果：`box-sizing` 一条都没生效，而页面看起来正常（因为当时没写死 width）
- 排查方法：`F12` → 元素 → 右侧「样式」面板，看规则**在不在**；被划掉 = 被更高优先级覆盖，看不到 = 语法错误被丢弃

### 2.9 本阶段还没搞懂的

- （待补）

---

## 阶段 3 · Git + 上线 — ✅ 完成（2026-11-27）

**产出**
- 本地 Git 仓库：`D:\my-blog\.git`，3 个提交
- 远程仓库：`https://github.com/ShigaDove/my-blog`
- **线上地址：`https://shigadove.github.io/my-blog/`**（GitHub Pages，公开可访问）

### 3.1 Git 不是 GitHub

| | 是什么 | 在哪 |
|---|---|---|
| **Git** | 版本控制工具 | 装在本地，不需要网络 |
| **GitHub** | 代码托管网站 | 云端，用来备份和分享 |

GitHub 只是"把本地 Git 仓库放到网上"，不用 GitHub 也能用 Git。

### 3.2 三个区

```
工作区              暂存区(.git/index)        本地仓库(.git/objects)
正在编辑的文件  →   git add 的待提交清单  →   git commit 的快照
```

- **暂存区存在的原因**：让你能挑着提交（改了 3 个文件只提交 2 个）
- 每个 commit = 「完整快照 + 作者 + 时间 + 说明 + **指向父节点的指针**」→ 历史是一条**链表**，分支只是**指向某个 commit 的指针**，`HEAD` 是"我现在在哪"

### 3.3 命令表

| 命令 | 作用 |
|---|---|
| `git status` | 最常用。看清现在在哪个阶段 |
| `git add .` | 全部改动放进暂存区 |
| `git commit -m "说明"` | 存成一个快照 |
| `git log --oneline` | 一行一个提交 |
| `git diff` | 工作区 vs 上次提交的差异 |
| `git push` | 推到远程 |
| `git restore 文件` | **用最后一次提交覆盖工作区**（救命） |

**颜色约定**（`git status`）：红 = 工作区有改动、没进暂存区；绿 = 已进暂存区。

### 3.4 撤销三连

| 情况 | 命令 |
|---|---|
| 改错了，还没 add | `git restore 文件名` |
| add 了但没 commit | `git restore --staged 文件名` |
| 已 commit | `git revert`（以后讲） |

### 3.5 踩坑：`fatal: not a git repository`

原因：**不在仓库目录里执行**。`PS C:\Users\he'ro>` 里跑 `git commit`，那里没有 `.git`。

- 教训：**敲命令前先看提示符的路径**
- 解法：每次先 `cd D:\my-blog`；或直接用 **VS Code 内置终端**（`` Ctrl+` `` 打开的会自动定位到当前文件夹）

### 3.6 踩坑：`Author identity unknown`

原因：跳过了 `git config` 步骤。Git 甚至猜了个 `he'ro@WIN-LGPSETKEEPD.(none)` —— 因为 Windows 用户名里带**单引号**，猜出来的地址是废的。这是「路径/用户名不要有单引号」的又一次实证。

修复：

```powershell
git config --global user.name "用户名"
git config --global user.email "邮箱"
git config --global init.defaultBranch main
git config --global --list     # 验证
```

### 3.7 踩坑：`.gitignore` 打不开，说"二进制文件"

原因：那个文件其实是个 **9KB 的 Office 文档**（建的时候误选了 Word/Excel 模板再改名）。证据是文件头：

```
D0 CF 11 E0 A1 B1 1A E1   ← OLE2 复合文档的魔数
```

**知识点：扩展名会撒谎，文件头（魔数）不会。**

| 魔数 | 类型 |
|---|---|
| `89 50 4E 47` | PNG |
| `FF D8 FF` | JPEG |
| `50 4B 03 04` | ZIP（.docx/.xlsx 也是） |
| `D0 CF 11 E0` | 老版 Office（.doc/.xls） |
| `EF BB BF` | UTF-8 带 BOM |

**建文本文件只在 VS Code 里建**；资源管理器右键要用「文本文档」，不要用 Office 模板。VS Code 右下角状态栏能看/改文件编码。

### 3.8 远程仓库与 push

```powershell
git remote add origin https://github.com/ShigaDove/my-blog.git
git remote -v        # 看有没有配错
git push -u origin main
```

- `git remote add` **不校验地址是否存在**，只记一行字符串；真正报错的是 push
- **第一次要 `-u`**：建立本地 `main` ↔ 远程 `origin/main` 的追踪关系，之后 `git push` 两个字就够
- GitHub 上**必须先建好空仓库**（名字要和 URL 一致），且**不要勾** README / .gitignore / license，否则 push 会被 `fetch first` 拒绝
- **密码认证已废**（2021-08 起）：用 GCM 浏览器登录，或 Personal Access Token（Settings → Developer settings → Tokens (classic) → 勾 `repo`）

### 3.9 GitHub Pages 上线

- 入口：仓库 **Settings → Pages**（新版藏在右上 **More ▾** 菜单里；也可以直达 `github.com/用户名/仓库/settings/pages`）
- 配置：**Deploy from a branch** → `main` → `/ (root)` → Save
- 地址：`https://用户名.github.io/仓库名/`，免费账号要求仓库 **Public**
- **每次 push 到 main 都会自动重新发布**（最原始的一层 CI/CD）
- 建议加一个空的 `.nojekyll` 文件：跳过 Jekyll 构建，否则以 `_` 开头的目录（如以后 Astro 的 `_astro/`）会被静默忽略

**为什么部署在子路径 `/my-blog/` 下也能跑**：因为全站用的是**相对路径**（`style.css`、`posts/hello.html`、`../style.css`）。绝对路径 `/style.css` 会去找域名根目录然后 404。

### 3.10 踩坑：push 了但网页没变

GitHub Pages 前面有 **CDN 缓存**，改完可能要等几分钟。也有浏览器缓存的因素 → `Ctrl+F5` 强制刷新 / 无痕窗口 / 等一会。**"代码对了但页面没变"先怀疑缓存。**

### 3.11 改 GitHub 用户名要改哪里

| 要改 | 命令 |
|---|---|
| 远程地址（**必须**） | `git remote set-url origin https://github.com/新名字/仓库.git` |
| 显示名（建议） | `git config --global user.name "新名字"` |
| 页脚署名等硬编码文本 | 手动改 |

**不用改**：邮箱（和用户名无关）、已提交的历史（GitHub 按 email 认人，旧 URL 会 302 重定向）、本地文件夹名。
**会变**：`用户名.github.io` 这个 Pages 地址。

### 3.12 本阶段还没搞懂的

- （待补）

---

<!-- 新阶段模板，复制这一小段往下加

## 阶段 N · 标题 — ⬜ 进行中

- 日期：
- 今天搞懂的概念：
- 还没搞懂的：

-->
