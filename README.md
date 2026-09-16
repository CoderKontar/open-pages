# 隐私政策站点（Edge 扩展「订单监控」）

发布给 Microsoft Edge Add-ons **Partner Center → Privacy → Privacy policy URL** 使用的静态页。

## 内容

| 文件 | 说明 |
|---|---|
| `index.html` | 隐私政策正文。中英双语同页（中文在前，英文在后）。**完全自包含**：无外部 CSS/JS/字体、无 Cookie、无第三方统计、无 `<script>` |

页面刻意保持零外部依赖——审核员或用户在断网/被墙环境下也能完整打开，且不会因为第三方 CDN 波动而出现「策略页打不开」。

## 发布为 GitHub Pages

**仓库必须是 public**：GitHub 免费账号的 Pages 只在公开仓库可用（私有仓库需 Pro 及以上）。

1. 在 GitHub 新建一个 public 仓库，例如 `order-monitor-privacy`
2. 把本目录推上去（`index.html` 放在仓库根目录）
3. 仓库 **Settings → Pages → Build and deployment**：
   - Source 选 `Deploy from a branch`
   - Branch 选 `main`，目录选 `/ (root)`，Save
4. 等 1–2 分钟，访问 `https://<用户名>.github.io/<仓库名>/`
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
