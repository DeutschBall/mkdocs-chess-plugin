# MkDocs Chess Plugin

在 MkDocs 的 Markdown 提示块里填写 FEN，即可生成可推演的中国象棋或国际象棋棋盘。棋盘和规则脚本随站点发布，适用于 MkDocs 内置主题和 Material 主题。

## 安装

从本项目源码目录安装。

```bash
python -m pip install .
```

在 `mkdocs.yml` 的插件列表中加入 `chess`。

```yaml
plugins:
  - search
  - chess
```

## 用法

```markdown
??? xiang "rnbakabnr/9/1c5c1/p1p1p1p1p/9/9/P1P1P1P1P/1C5C1/9/RNBAKABNR w - - 0 1"

??? chess "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1"
```

`???` 默认折叠，`???+` 默认展开，`!!!` 始终显示。块前后留一个空行，说明文字可在块内缩进四个空格书写。旧写法 `board` 按 FEN 的行数识别棋种，八行为国际象棋，十行为中国象棋。

折叠块标题显示当前行棋方的棋子图标。中国象棋的标题是「局面」，国际象棋的标题是「Position」。

中国象棋红方在下，国际象棋白方在下。可以点选棋子再点合法落点，也可以用鼠标或手指拖到落点。键盘用方向键和 Enter 操作。每个棋盘的「上一步」和「下一步」独立生效，回到上一步后走出新变化会清除后续着法。国际象棋支持易位、吃过路兵和选择升变棋子。

按住鼠标右键从任意一格拖到另一格，可以画出带编号的推演箭头。红方或白方为绿色，黑方为紫色。从空位起笔时，颜色取决于当前行棋方。右键点击箭头尖端或编号可删除单条，也可以点「清除箭头」清空。走棋、上一步和下一步都会清空当前标注。

两种棋类都支持六字段 FEN，或只写棋子布局与行棋方。两字段格式的易位权和过路兵字段默认为 `-`，计数默认为 `0 1`。中国象棋也接受 `H/E` 作为 `N/B` 的别名。

点「导出 FEN」可复制棋盘当前局面的六字段 FEN。浏览器拒绝写入剪贴板时，插件会显示并选中 FEN 文本供手动复制。

构建时会检查 FEN 格式并生成 SVG。浏览器启用 JavaScript 时可推演；格式正确但规则引擎拒绝的局面会保留棋盘并提示原因。格式错误会指出所在页面及原因。

## 配置走棋音效

在 `mkdocs.yml` 中为普通走子和吃子指定音频文件。路径相对于配置文件，支持 MP3、Ogg 和 WAV。素材由站点提供，MkDocs 构建时将其复制到站点。

```yaml
plugins:
  - search
  - chess:
      sounds:
        move: sounds/move.mp3
        capture: sounds/capture.mp3
```

## 配置棋盘与棋子样式

在 `mkdocs.yml` 注册素材清单，路径相对于配置文件。每个棋种可以分别指定默认棋盘和棋子。

```yaml
plugins:
  - search
  - chess:
      themes:
        wood: themes/wood/theme.json
        ivory: themes/ivory/theme.json
      defaults:
        xiang:
          board: wood
          pieces: ivory
```

文章也可以用 `board-<名称>` 和 `pieces-<名称>` 覆盖默认值。例如 `??? xiang board-wood pieces-ivory "FEN"`。使用 `board-default` 或 `pieces-default` 可以选回插件自带样式。

素材清单可以只提供 `board` 或只提供 `pieces`，也可以同时提供两项。图片与清单放在同一目录，支持 SVG、PNG、JPEG、GIF 和 WebP。插件在构建时把配置中的素材复制到站点。

```json
{
  "game": "xiang",
  "board": {
    "image": "wood.jpg",
    "size": [665, 737],
    "origin": [39.5, 39.5],
    "step": [73, 73]
  },
  "pieces": {
    "scale": 1,
    "offset": [0, 0],
    "images": {
      "K": "RK.gif", "A": "RA.gif", "B": "RB.gif", "N": "RN.gif",
      "R": "RR.gif", "C": "RC.gif", "P": "RP.gif",
      "k": "BK.gif", "a": "BA.gif", "b": "BB.gif", "n": "BN.gif",
      "r": "BR.gif", "c": "BC.gif", "p": "BP.gif"
    }
  }
}
```

`size` 是棋盘图宽高，`origin` 是左上落点中心，`step` 是横纵落点间距，单位均为图内像素。国际象棋的落点在格子中央，中国象棋在交叉点上。棋子的 `scale` 按落点间距缩放，`offset` 以落点间距为单位，正值向右和向下移动。两项均可省略。大写和小写分别对应红或白方与黑方，`images` 需覆盖该棋种所有 FEN 棋子字母。站点自行配置的素材在插件安装包之外。

## 扩展棋种

Markdown 块的棋种名来自 `markdown.py` 中的 `BOARD_TYPES`，每项注册布局尺寸、FEN 字段校验和 SVG 渲染函数。新增棋种时，为同名项接入 `board.mjs` 的规则适配器。比如将来注册 `shogun` 后，可用 `??? shogun "局面编码"`。本版本注册了 `chess` 和 `xiang`。

## 打包

在此目录运行以下命令生成源码包和 wheel，并校验发布元数据。

```bash
python -m pip install build twine
python -m build
python -m twine check dist/*
```

国际象棋规则使用 chess.js 1.4.0，BSD-2-Clause 许可文本随包分发。中国象棋规则随包提供，源自博客的象棋研究室。
