# GLAF4 Dev 公开知识站

**在线文档站（GitHub Pages）：** <https://d00000-archt.github.io/glaf4-dev-docs/>

本仓库托管 `glaf4-dev` 开发行为技能族的独立公开知识站静态产物，集中提供环境与多宿主接入、自然语言统一分诊、七种开发闭环模式、只读 AI 代码审查、公开入口参考、能力边界说明与故障排查指南。日常浏览站点无需安装任何命令行工具。

<!-- release-skill:capability:safe-first-command -->
> **安全起点：** 浏览文档可直接打开线上地址或本地 `index.html`。维护者在源工作区核查站点与发布状态时，可先运行只读命令 `python3 tools/check-public-site.py` 或 `npx release-skill help --json`，两项命令均不修改文件，也不触发远端写入。

<!-- release-skill:capability:external-write-boundary -->
> **外部写入边界：** 本仓库仅承载由源工作区 `docs/public/rendered/` 投影出的纯静态 HTML、CSS、JavaScript、SVG 与站内搜索索引。更新远端分支、创建 `site-v*` 标签、发布 GitHub Release 及修改 GitHub Pages 设置均须获得当次任务的明确授权。

## 本地浏览与站点核验

直接在浏览器中打开根目录 `index.html`（源工作区内为 `docs/public/rendered/index.html`）即可阅读。如需在本地模拟静态服务访问行为，可在站点根目录启动只读 HTTP 服务：

```sh
python3 -m http.server 8000
```

页面与 `modern` 主题资源全部使用相对路径并携带内容 SHA-256 指纹参数，不依赖外部 CDN 或运行时打包器。维护者在源工作区核查发布工具参数与门禁状态时，可运行以下最小只读命令：

```sh
python3 tools/check-public-site.py
npx release-skill help --json
```

## 内容维护与联合发布约定

站点页面由源工作区 `docs/public/site/pages.json` 与同名 Markdown 原稿经精确版本的 `skill-family-doc-render`（`modern` 主题）确定性生成。需要修订文字、图表或导航结构时，请回到源工作区修改 `docs/public/site/` 原稿、重新渲染并刷新覆盖锁，切勿直接手改生成的 HTML 文件。

每次正式发布时，本站点单元与主插件单元读取同一权威版本源并联合冻结快照：先跑通完整性钉扎、公开站防泄漏与多视口版面检查，再经批准推送至本仓库 `main` 分支并创建对应的 `site-v*` 标签与 GitHub Release。更多维护与发版细节请参阅站内 `maintenance.html` 与 `release.html`。

## 常见故障排查

- **站点或发布门禁阻断（`GATE_FAILED` / `CONFIG_INVALID`）**：查看 `python3 tools/check-public-site.py` 输出的具体检查项（入口完整性钉扎、覆盖锁过期、渲染基线漂移、断链或私有字面量命中），在源工作区修正原稿并重新渲染、刷新 `site-coverage-lock.json` 后重试。
- **发布中断处于 `PARTIAL` 状态**：使用独立空缓存执行 `release-skill reconcile` 按既有运行证据幂等恢复，恢复后再运行 `release-skill verify`，切勿强推覆盖远端分支或标签。
- **本地预览样式或搜索不可用**：确认 `index.html` 与 `assets/theme.css`、`assets/site.js`、`assets/search-index.json`、`assets/mark.svg` 及 `assets/diagrams/` 位于同一目录层级，并通过只读本地 HTTP 服务打开页面。
