# MkDocs Chess Plugin

mkdocs博客的象棋插件。允许玩家使用FEN码绘制棋盘局面。


## 安装

从PYPI库安装：

```bash
pip install mkdocs-chess-plugin
```

从本项目源码目录安装。

```bash
python -m pip install .
```

在 `mkdocs.yml` 配置文件的插件列表中启用 `chess`。

```yaml
plugins:
  - chess
```

## 用法

>确保浏览器启用javascript。
>确保博客使用mkdocs框架搭建。

棋盘块与mkdocs自带的折叠块语法基本相同。

可以使用FEN码规定棋盘初始状态。

```markdown
??? xiang "rnbakabnr/9/1c5c1/p1p1p1p1p/9/9/P1P1P1P1P/1C5C1/9/RNBAKABNR w - - 0 1"
```

```markdown
??? chess "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1"
```

可以在棋盘上点选或者拖动棋子走子。

支持鼠标右键拖动画线推演功能。

支持上一步/下一步功能。

支持导出实时推演局面的FEN码。

## 自定义配置

### 配置界面语言

`language` 同时应用于中国象棋和国际象棋的块标题、按钮、走棋提示和无障碍标签。

```yaml
plugins:
  - search
  - chess:
      # 候选：zh（中文）、en（英语）、ru（俄语）、vi（越南语）
      #       ko（韩语）、ja（日语）、de（德语）、fr（法语）
      language: zh
```


### 配置走棋音效

```yaml
plugins:
  - search
  - chess:
      # 候选：chesscom（Chess.com）、tiantian（天天象棋）
      sounds: chesscom
```

省略 `sounds` 时选用 `chesscom`。两套音效各包含落子和吃子，选好名称即可。
站点维护者将有使用权限的音效包放到配置文件旁的 `sound-packs/chesscom/` 或 `sound-packs/tiantian/`，包内包含 `move.ogg` 和 `capture.ogg`。站点没有对应的音效包时，棋盘以静音方式运行。

### 配置棋盘与棋子样式


```yaml
plugins:
  - search
  - chess:
      defaults:
        xiang:
          # board 候选：default（经典）、wood（木纹）、green（青绿）
          board: wood
          # pieces 候选：default（经典）、ivory（象牙白）、ink（墨黑）、wood（木色）
          pieces: ivory
        chess:
          # board 候选：default（经典）、wood（木纹）、green（绿白格）
          board: green
          # pieces 候选：default（经典）、ivory（象牙白与深棕）、ink（黑白）
          pieces: ivory
```

文章中也可以用 `board-<名称>` 和 `pieces-<名称>` 临时覆盖站点默认样式。

```markdown
??? xiang board-green pieces-ink "rnbakabnr/9/1c5c1/p1p1p1p1p/9/9/P1P1P1P1P/1C5C1/9/RNBAKABNR w - - 0 1"
```



## 扩展棋种

Markdown 块的棋种名来自 `markdown.py` 中的 `BOARD_TYPES`，每项注册布局尺寸、FEN 字段校验和 SVG 渲染函数。

新增棋种时，为同名项接入 `board.mjs` 的规则适配器。

比如将来注册 `shogun` 后，可用 `??? shogun "局面编码"`。

本版本注册了 `chess` 和 `xiang`。

## 打包

在此目录运行以下命令生成源码包和 wheel，并校验发布元数据。

```bash
python -m pip install build twine
python -m build
python -m twine check dist/*
```
