# LingBot-World 2.0 论文卡片实施计划

> **面向执行代理：** 必须使用子技能 `superpowers:subagent-driven-development`（推荐）或 `superpowers:executing-plans`，逐项执行本计划。所有步骤使用复选框（`- [ ]`）跟踪。

**目标：** 在个人主页 Selected Publications 列表首位新增 LingBot-World 2.0 论文卡片，并使用项目仓库中的本地 teaser 图片。

**架构：** 该站点是已经生成的静态 HTML，因此直接在 `index.html` 中沿用现有 publication card 结构，不新增组件、样式或脚本。teaser 固定到上游提交 `0c1be1bbbe271c45f36bbd8203ef837a5a8b516f` 并保存到本地；使用一个位于 `/tmp` 的 Python 标准库契约测试完成修改前失败、修改后通过的验证闭环。

**技术栈：** HTML5、现有 al-folio/Bootstrap 样式、Python 3 标准库、Git、curl。

---

## 文件结构

- 修改：`index.html`——在 Selected Publications 列表首位加入卡片。
- 新增：`assets/teaser/lingbot-world-v2.png`——从项目仓库复制的固定版本 teaser。
- 临时测试：`/tmp/test_lingbot_world_card.py`——验证卡片结构、顺序、内容、链接和 PNG 文件；不提交到仓库。

### 任务 1：建立失败的页面契约测试

**文件：**
- 新建：`/tmp/test_lingbot_world_card.py`
- 检查：`index.html`
- 检查：`assets/teaser/lingbot-world-v2.png`

- [ ] **步骤 1：使用 `apply_patch` 新建临时测试文件**

```python
from pathlib import Path

ROOT = Path("/Users/louis/Documents/yichonglu.github.io")
HTML_PATH = ROOT / "index.html"
IMAGE_PATH = ROOT / "assets/teaser/lingbot-world-v2.png"

html = HTML_PATH.read_text(encoding="utf-8")
card_id = 'id="Gao2026Arxiv"'

count = html.count(card_id)
assert count == 1, f"expected one LingBot card, found {count}"
card_position = html.index(card_id)
assert card_position < html.index('id="Lu2025Arxiv"'), "LingBot card is not first"

card_start = html.rindex("<li>", 0, card_position)
card_end = html.index("</li>", card_position)
card = html[card_start:card_end]

expected_tokens = [
    "Infinite Worlds with Versatile Interactions",
    'src="/assets/teaser/lingbot-world-v2.png"',
    "arXiv 2026",
    "<em>Lu, Yichong</em>",
    'href="https://arxiv.org/pdf/2607.07534"',
    'href="https://github.com/Robbyant/lingbot-world-v2"',
    'href="https://technology.robbyant.com/lingbot-world-v2"',
    "We present LingBot-World 2.0",
]
for token in expected_tokens:
    assert token in card, f"missing card token: {token}"

expected_authors = [
    "Gao, Zelin", "Wang, Qiuyu", "Zhu, Jiapeng", "Chen, Jingye",
    "Liu, Zichen", "Bai, Qingyan", "Wang, Jiahao", "Yuan, Yufeng",
    "Wang, Hanlin", "Lu, Yichong", "Cheng, Ka Leong", "Zhang, Haojie",
    "Gao, Jian", "Feng, Tianrui", "Liu, Yuzheng", "Yao, Yao",
    "Xu, Yinghao", "Zhu, Xing", "Shen, Yujun", "Ouyang, Hao",
]
for author in expected_authors:
    assert author in card, f"missing author: {author}"

image = IMAGE_PATH.read_bytes()
assert image.startswith(b"\x89PNG\r\n\x1a\n"), "teaser is not a PNG"
assert len(image) > 1024, "teaser is unexpectedly small"

print("PASS: LingBot-World card contract")
```

- [ ] **步骤 2：运行测试并确认它在修改前失败**

运行：

```bash
python3 /tmp/test_lingbot_world_card.py
```

预期：退出码非 0，并显示 `AssertionError: expected one LingBot card, found 0`。

### 任务 2：添加 teaser 和论文卡片

**文件：**
- 新增：`assets/teaser/lingbot-world-v2.png`
- 修改：`index.html`
- 测试：`/tmp/test_lingbot_world_card.py`

- [ ] **步骤 1：下载固定提交中的 teaser 图片**

运行：

```bash
curl -fsSL https://raw.githubusercontent.com/Robbyant/lingbot-world-v2/0c1be1bbbe271c45f36bbd8203ef837a5a8b516f/assets/teaser.png -o assets/teaser/lingbot-world-v2.png
```

预期：命令退出码为 0，目标文件非空。

- [ ] **步骤 2：使用 `apply_patch` 将以下完整卡片插入 `<ol class="bibliography">` 后、原 `Lu2025Arxiv` 卡片之前**

```html
          <li>
        <div class="row">
          <div class="col-md-3">

            <div class="img-fluid rounded">
              <img src="/assets/teaser/lingbot-world-v2.png" alt="Infinite Worlds with Versatile Interactions" style="width: 100%;">
            </div>

          </div>

          <div id="Gao2026Arxiv" class="col-md-9">
                <div class="title">Infinite Worlds with Versatile Interactions</div>
                      <div class="author">

                          <a target="_blank" rel="noopener noreferrer">Gao, Zelin</a>,

                          <a target="_blank" rel="noopener noreferrer">Wang, Qiuyu</a>,

                          <a target="_blank" rel="noopener noreferrer">Zhu, Jiapeng</a>,

                          <a target="_blank" rel="noopener noreferrer">Chen, Jingye</a>,

                          <a target="_blank" rel="noopener noreferrer">Liu, Zichen</a>,

                          <a target="_blank" rel="noopener noreferrer">Bai, Qingyan</a>,

                          <a target="_blank" rel="noopener noreferrer">Wang, Jiahao</a>,

                          <a target="_blank" rel="noopener noreferrer">Yuan, Yufeng</a>,

                          <a target="_blank" rel="noopener noreferrer">Wang, Hanlin</a>,

                          <em>Lu, Yichong</em>,

                          <a target="_blank" rel="noopener noreferrer">Cheng, Ka Leong</a>,

                          <a target="_blank" rel="noopener noreferrer">Zhang, Haojie</a>,

                          <a target="_blank" rel="noopener noreferrer">Gao, Jian</a>,

                          <a target="_blank" rel="noopener noreferrer">Feng, Tianrui</a>,

                          <a target="_blank" rel="noopener noreferrer">Liu, Yuzheng</a>,

                          <a target="_blank" rel="noopener noreferrer">Yao, Yao</a>,

                          <a target="_blank" rel="noopener noreferrer">Xu, Yinghao</a>,

                          <a target="_blank" rel="noopener noreferrer">Zhu, Xing</a>,

                          <a target="_blank" rel="noopener noreferrer">Shen, Yujun</a>,

                        and <a target="_blank" rel="noopener noreferrer">Ouyang, Hao</a>

                      </div>

                      <div class="periodical">

                        arXiv 2026

                      </div>

                      <div class="links">

                        <a class="abstract btn btn-sm z-depth-0" role="button">Abs</a>

                        <a href="https://arxiv.org/pdf/2607.07534" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">PDF</a>

                        <a href="https://github.com/Robbyant/lingbot-world-v2" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Code</a>

                        <a href="https://technology.robbyant.com/lingbot-world-v2" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener noreferrer">Website</a>

                      </div>

                      <!-- Hidden abstract block -->

                      <div class="abstract hidden">
                        <p>We present LingBot-World 2.0 (also known as LingBot-World-Infinity), an advanced iteration of LingBot-World featuring four distinct upgrades. (1) Our model achieves an unbounded interaction horizon while maintaining consistent output quality, benefiting from a carefully crafted causal pretraining paradigm. (2) Through distilling a real-time variant from the base model, our system guarantees rapid response time, sufficient to drive 720p video streams at 60 fps. (3) Compared to the previous version, this update introduces highly diverse interactive elements, comprising a broader spectrum of actions (e.g., attacking, archery, spell-casting, and shooting) alongside a richer variety of text-driven events. (4) We pioneer the integration of an agentic harness within the domain of world modeling, wherein a pilot agent is tasked with planning and executing character behaviors, while a director agent is responsible for synthesizing novel environmental elements as the scene progresses. Additionally, to facilitate a shared experience, we develop an interface that permits multiple players to simultaneously immerse themselves in this vivid world simulator. We pair our primary 14B model with a lightweight 1.3B counterpart, which supports effortless deployment on a single GPU.</p>
                      </div>

          </div>
        </div>
        </li>
```

- [ ] **步骤 3：重新运行契约测试并确认通过**

运行：

```bash
python3 /tmp/test_lingbot_world_card.py
```

预期：退出码为 0，并显示 `PASS: LingBot-World card contract`。

- [ ] **步骤 4：检查文件格式和补丁质量**

依次运行：

```bash
file assets/teaser/lingbot-world-v2.png
git diff --check
git status --short
```

预期：`file` 将图片识别为 PNG；`git diff --check` 无输出且退出码为 0；状态中仅出现 `index.html` 和 `assets/teaser/lingbot-world-v2.png` 的实现改动。

- [ ] **步骤 5：提交实现**

```bash
git add index.html assets/teaser/lingbot-world-v2.png
git commit -m "feat: add LingBot-World publication"
```

预期：提交成功，且提交仅包含上述两个文件。

### 任务 3：执行最终页面验证

**文件：**
- 验证：`index.html`
- 验证：`assets/teaser/lingbot-world-v2.png`

- [ ] **步骤 1：运行完整契约测试和仓库检查**

依次运行：

```bash
python3 /tmp/test_lingbot_world_card.py
git diff --check HEAD^ HEAD
git show --stat --oneline HEAD
git status --short --branch
```

预期：契约测试通过；补丁检查无错误；最新提交只包含 `index.html` 和 teaser；工作树干净。

- [ ] **步骤 2：启动本地静态服务器并进行视觉检查**

运行：

```bash
python3 -m http.server 4173
```

在浏览器打开 `http://127.0.0.1:4173/`，确认 LingBot-World 卡片位于 Selected Publications 首位，图片无拉伸或破损，作者和按钮正常换行，点击 `Abs` 能展开/收起摘要。

- [ ] **步骤 3：验证本地 teaser 响应并停止服务器**

在另一个终端运行：

```bash
curl -I http://127.0.0.1:4173/assets/teaser/lingbot-world-v2.png
```

预期：返回 `HTTP/1.0 200 OK` 或 `HTTP/1.1 200 OK`，并包含 `Content-type: image/png`。完成后停止静态服务器。
