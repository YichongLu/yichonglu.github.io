# LingBot-World 2.0 论文卡片设计

## 目标

将 LingBot-World 2.0 添加到主页 **Selected Publications** 列表的首位，同时保持网站现有的视觉样式和交互方式。

## 权威信息来源

以公开的 `Robbyant/lingbot-world-v2` 仓库和 arXiv 记录 `2607.07534` 为权威信息来源：

- 论文标题：**Infinite Worlds with Versatile Interactions**
- 项目名称：**LingBot-World 2.0** / **LingBot-World-Infinity**
- 发表信息标签：**arXiv 2026**
- 项目主页：`https://technology.robbyant.com/lingbot-world-v2`
- 代码仓库：`https://github.com/Robbyant/lingbot-world-v2`
- 论文链接：`https://arxiv.org/pdf/2607.07534`
- 缩略图来源：项目仓库中的 `assets/teaser.png`
- 作者：采用 arXiv 中的作者顺序，并在主页中突出显示 Yichong Lu

## 已考虑的方案

1. **仅新增主页论文卡片——已批准。** 新增一张与现有条目一致的卡片，不增加额外导航或页面。这是改动最小的方案，也能保持列表按时间排序。
2. **新增主页卡片和独立项目页面。** 这种方案能为演示内容提供更多空间，但会与外部项目主页重复，并带来不必要的维护成本。
3. **新增主页卡片和 News 条目。** 这种方案能提高项目的可见度，但当前需求仅要求新增项目，并未要求发布新闻公告。

## 已批准的设计

- 将上游 teaser 原样复制到 `assets/teaser/lingbot-world-v2.png`，避免主页依赖 GitHub 原始文件的可用性。
- 在 `index.html` 当前第一篇论文之前插入一个新的 `<li>`。
- 沿用现有的三栏/九栏卡片结构和按钮样式。
- 使用唯一的卡片 ID：`Gao2026Arxiv`。
- 展示准确的论文标题、完整作者列表和 `arXiv 2026` 发表信息标签。
- 使用 `<em>` 突出显示 `Lu, Yichong`；其他作者使用不带链接地址的普通作者标签，不虚构个人主页 URL。
- 按顺序提供现有样式的 `Abs`、`PDF`、`Code` 和 `Website` 按钮。
- 使用 arXiv 摘要填充隐藏的摘要区块。
- 使用与论文标题一致、具有描述性的缩略图替代文本。
- 不在 `projects/` 下新增独立页面，不增加 News 条目、CSS 或 JavaScript。

## 验证方式

- 确认复制后的缩略图是有效且非空的 PNG 文件。
- 解析 `index.html`，确认其中恰好存在一个 `Gao2026Arxiv` 卡片，并且它位于原先第一篇论文之前。
- 确认卡片包含预期标题、四个链接、`arXiv 2026`、突出显示的 `Lu, Yichong` 和本地 teaser 路径。
- 确认仓库差异仅涉及设计文档、`index.html` 和 teaser 图片。

## 验收标准

主页在 Selected Publications 的首位展示 LingBot-World 2.0，其视觉样式与现有条目一致；缩略图能够从本地正常加载；PDF、代码仓库、项目主页和摘要控件均存在且内容正确。
