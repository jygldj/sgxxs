# 三国续-数字卜易传 · 网站说明（README）

> 最后更新：2026-08-15
> 维护人：jygldj ｜ 本地工作目录：`F:\github-dx\sgx`

---

## 一、站点定位

《三国续-数字卜易传》是「道玄文集」的**小说分站**，独立部署于 `https://sgx.pages.dev`。
纯静态站点，无后端（不依赖字典 / AI 等 Pages Functions）。

---

## 二、目录结构（仓库根 `F:\github-dx\sgx\`）

```
F:\github-dx\sgx\
├── index.html            # 扉页（翻书动画封面）
├── index1.html           # 阅读主页：按卷分章目录 + 正文渲染 + 卷导航
├── search.html           # 全文搜索（站内跨卷搜小说）
├── build.html            # 文章更新工具页（由 更新网站.bat 打开）
├── jianjie.html          # 关于作者 / 版权页
├── articles.js           # 文章索引数据（由更新工具自动生成，禁止手改）
├── build-core.js         # 更新工具核心逻辑（build.html 调用）
├── render.js             # 正文渲染器 + 夜间模式 / 字号 / 分享工具
├── reader.js             # 阅读器主控：按卷导航 / 搜索 / 文章导航 / 侧边栏
├── site-config.js        # 站点配置（SITE_BASE = https://sgx.pages.dev）
├── sw.js                 # Service Worker（离线缓存；版本号 sgx-v1）
├── style.css / cover.css # 样式
├── articles/             # 小说源文件（.md），每卷一个文件
└── 更新网站.bat          # 双击打开 build.html 的本地启动器
```

> 本分站**不含**新华字典（`dict.html` / `dict.js` / `functions/api/dict.js`）——那是主站道玄文集的能力，小说站无需查字。若仓库里仍见这些文件，属历史遗留，未被任何页面引用，可手动删除。

---

## 三、文章格式（articles/ 下的 .md）

每卷一个 `.md` 文件，命名 `00N-卷名.md`，顶部两行固定格式：

```
# 卷名简称
>  |卷X
```

- 第 1 行 `# ` 后为文章（卷）标题；阅读页会去掉文件名里的「卷X·」前缀，展示简称。
- 第 2 行 `>  |卷X` 为卷标记：`|` 前留空、`|` 后为卷名（卷一 ~ 卷八）。构建工具（`build-core.js`）据此提取 `juan` 字段，用于阅读页分卷导航。
- 其余为小说正文（支持 Markdown：`##`、`###`、`![图](路径)` 等原样保留，由 `render.js` 渲染）。

示例（卷三）：
```
# rw7风暴：凡错必认
>  |卷三

（正文……）
```

---

## 四、构建与更新

1. 把写好的小说 `.md` 放进 `articles/` 文件夹；
2. 双击 `更新网站.bat` → 在本页点「开始更新」（格式写错会点名提醒）；
3. 点「打开 GitHub Desktop」→ Commit → Push origin，约 1 分钟网站更新生效。

`articles.js` 由 `build-core.js` 自动生成，数据结构含 `juan` 卷字段，**禁止手工编辑**。

---

## 五、阅读页卷导航

`reader.js` 读取 `articlesData` 中的 `juan` 字段，在导航栏动态生成「卷一 ~ 卷八」按钮；
点击切换卷，左侧目录仅显示该卷章节；搜索为全站跨卷搜索。

---

## 六、部署

- **静态托管**：Cloudflare Pages（项目 `sgx`）直接托管，线上 `https://sgx.pages.dev`；
- 无后端 Functions，无需配置 KV / 环境变量；
- 推送 `main` 分支即自动部署。

---

## 七、与姊妹站关系

| 站点 | 线上地址 | 说明 |
|---|---|---|
| 道玄文集（主站） | `dxwj.pages.dev` | 诗词散文 |
| 三国续-数字卜易传（本分站） | `sgx.pages.dev` | 小说 |
| 增删卜易 | `dxzsby.pages.dev` | 六爻占卜独立子系统 |
| 三省轩主文集 | `sxxz.pages.dev` | 诗词散文姊妹站 |
