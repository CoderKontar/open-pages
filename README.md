# 静态公开页（Edge 扩展「订单监控」）

发布给 Microsoft Edge Add-ons **Partner Center** 使用的静态页，本站点托管两个页面：

| 页面 | URL | 用途 |
|---|---|---|
| 隐私政策 | `https://coderkontar.github.io/open-pages/` | 填 Partner Center → Privacy → **Privacy policy URL** |
| 功能演示录像 | `https://coderkontar.github.io/open-pages/video/` | 填 Partner Center → **Submission options → Notes for certification**（作为 1.3.1 Product is Testable 的补充证明材料） |

> **隐私政策 URL 已经提交过，不要动它。** 视频页放在 `video/` 子目录，根路径的 URL 不变，已提交的商店链接不会失效。

## 内容

| 文件 | 说明 |
|---|---|
| `index.html` | 隐私政策正文。中英双语同页（中文在前，英文在后）。**完全自包含**：无外部 CSS/JS/字体、无 Cookie、无第三方统计、无 `<script>` |
| `video/index.html` | 演示录像页。中英双语（英文在前，面向审核员）。自包含，仅一段内联脚本用于章节跳转 |
| `video/video.mp4` | 录像本体（29.6 MB） |
| `.nojekyll` | 关掉 Jekyll 处理。避免将来加入 `_` 开头的目录/文件被静默忽略，也省一次构建 |
| `.gitignore` | 挡掉 `.DS_Store` 与录屏原始素材（`.mov`、`raw/`） |

隐私政策页刻意保持零外部依赖——审核员或用户在断网/被墙环境下也能完整打开，且不会因为第三方 CDN 波动而出现「策略页打不开」。

## 放入录像

1. 把录像转码成 **H.264 + yuv420p + faststart**（`+faststart` 必须加，否则要先下完整个文件才开播）：
   ```bash
   ffmpeg -i raw.mov -vf "scale=1920:-2" -c:v libx264 -profile:v high -pix_fmt yuv420p \
          -crf 23 -preset slow -c:a aac -b:a 128k -movflags +faststart video.mp4
   ```
2. 存成 `video/video.mp4`（文件名要与 `video/index.html` 里的 `<source src="video.mp4">` 一致）
3. 确认体积 **< 100 MB**（GitHub 单文件推送硬限，>50 MB 会告警）
4. 提交前自查：订单行的**客户姓名 / 手机号 / 收货地址必须已打码**，且没有拍到密码输入过程

**绝对不要用 Git LFS。** GitHub Pages **不解析 LFS 指针**，页面只会拿到一个 130 字节的文本文件，视频永远播不出来。文件直接普通提交进 Git。

### 当前录像（2026-09-26）

| 项 | 值 |
| --- | --- |
| 文件 | `video/video.mp4` |
| 时长 | 73.6 秒 |
| 画面 | 1728 × 1080，30 fps |
| 编码 | H.264 + AAC，3.2 Mbps |
| 体积 | 29.6 MB |
| faststart | ✅（moov 在前，可边下边播） |

### 章节时间码

`video/index.html` 的 `Chapters` 列表是**按实际录像逐帧核对后**定的，直接可用：

| 时间 | 内容 |
| --- | --- |
| 0:00 | Edge 扩展管理页（`edge://extensions`） |
| 0:09 | 工具栏扩展面板，「订单监控」已固定 |
| 0:13 | 打开扩展弹窗：「启用监控」此时为**已暂停**，轮询间隔 30 分钟，两条备注未配置 |
| **0:18** | **未登录打开宿主订单页 → 跳登录页，右下角出现「请先登录」胶囊** ← 免登录可验证点，最关键的一镜 |
| 0:29 | 在弹窗中开启「启用监控」→ 浮球出现 |
| 0:39 | 展开浮层：待分派订单 / 今日分派订单 + 订单表格 |
| 0:46 | 设置抽屉：人员配置（分析类型 → 处理人） |
| 0:52 | 复制字段配置 |
| 0:56 | 表头列拖拽排序 |
| 1:02 | 关闭抽屉回面板 |
| 1:08 | 回到弹窗：「启用监控」显示**运行中** |

改时间码只改 `data-t`（单位秒），和旁边显示的 `0:00` 文本相互独立。

### 已知缺口与处置（2026-09-26）

| # | 缺口 | 处置 |
| --- | --- | --- |
| 1 | 没有「关闭监控 → 浮层消失」这一镜 | **决定忽略，不补录。** 0:29 已拍到「开 → 浮层出现」的正向过程，弹窗内也有文字提示 |
| 2 | 没有演示「一键复制字段」的实际动作（演示账号两张表都是 `No data`，无从复制） | **已补说明**：视频页「About this recording」b 条 + 认证说明 §3 `ABOUT THE RECORDING` 段 |
| 3 | 「录入备注 / 派单备注」未配置 → 列表不出现「录 / 派」按钮 | **已补说明**：视频页同节 a 条 + 认证说明 §3 同段落（口径：配置状态，非故障） |

⚠️ **维护陷阱**：视频页与认证说明里那两段「刻意未展示」的说明，**前提是「演示账号为空 + 两条备注未配置」**。
将来若换成有数据的账号重录，或先把备注配好再录，**必须把这两段删掉** ——
否则页面在陈述与画面不符的事实，比不解释更糟。

### 脱敏现状

- ✅ 客户姓名 / 手机号 / 收货地址：已打码
- ✅ 登录页账号、密码输入框：已打码，且未真正提交登录
- ✅ 页面右下角有「DEMO - customer data redacted」水印
- ⚠️ **设置抽屉的「处理人」下拉里有真实同事姓名**（格式为「分析类型-姓氏+职称」），未打码。发给微软前建议一并模糊处理，或确认无妨。
  （具体姓名不写进本文件 —— **本仓库是公开的**，见录像 0:52 附近。）

## 发布为 GitHub Pages

**仓库必须是 public**：GitHub 免费账号的 Pages 只在公开仓库可用（私有仓库需 Pro 及以上）。

1. 仓库：`https://github.com/CoderKontar/open-pages`（已创建）
2. 把本目录推上去（`index.html` 放在仓库根目录）
3. 仓库 **Settings → Pages → Build and deployment**：
   - Source 选 `Deploy from a branch`
   - Branch 选 **`master`**，目录选 `/ (root)`，Save

   > ⚠️ 分支名是 **`master`**，不是 `main`。本仓库初始化的就是 `master`，
   > 选 `main` 会得到一个永远 404 的 Pages 站点。
4. 等 1–2 分钟，访问 `https://coderkontar.github.io/open-pages/`
5. 把这个 URL 填进 Partner Center 的 Privacy policy URL

## 更新政策时

直接改 `index.html` 并更新页面顶部的「最后更新」日期，然后重新 push 即可——URL 不变，已提交的商店链接不会失效。

**重要**：本页内容必须与扩展的**实际行为**、以及 Partner Center 隐私页里的勾选**逐项一致**。
将来只要加入任何遥测、上报或云同步功能，必须同时改这三处，否则属违规。

## 联系方式

页面第十节（及英文第 10 节）留的是 `kontar_wk@163.com`。
若今后改用公司支持邮箱，改 `index.html` 里两处 `mailto:` 链接即可。

## 维护提醒

- 联系人/邮箱变更、数据范围变更时，记得同步更新**页面顶部**的「最后更新」日期
- 本目录是独立仓库，不要把它并进扩展源码仓库——避免扩展源码被一并公开
- `video/index.html` 顶部有 `<meta name="robots" content="noindex">`：视频页不想被搜索引擎收录，
  但**不加任何访问限制**——审核员必须能直接打开

