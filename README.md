# MkDocs Chess Plugin

An MkDocs plugin for embedding playable Xiangqi (Chinese chess) and chess positions from FEN.

## Installation

Install the plugin from PyPI.

```bash
pip install mkdocs-chess-plugin
```

To install from source, run the following command from the repository root.

```bash
python -m pip install .
```

Add `chess` to the `plugins` list in `mkdocs.yml`.

```yaml
plugins:
  - chess
```

## Usage

Put the starting FEN in the title of an MkDocs details block. Use `xiang` for Xiangqi or `chess` for chess.

```markdown
??? xiang "rnbakabnr/9/1c5c1/p1p1p1p1p/9/9/P1P1P1P1P/1C5C1/9/RNBAKABNR w - - 0 1"
```

```markdown
??? chess "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1"
```

Enable JavaScript to play moves by selecting or dragging pieces. Right-drag to draw arrows, use **Previous move** and **Next move** to replay moves, and select **Export FEN** to copy the current position.

## Configuration

### Interface language

`language` sets the board headings, buttons, move prompts, and accessibility labels for both games.

```yaml
plugins:
  - search
  - chess:
      # Options: zh (Chinese), en (English), ru (Russian), vi (Vietnamese),
      #          ko (Korean), ja (Japanese), de (German), fr (French)
      language: en
```

### Move sounds

```yaml
plugins:
  - search
  - chess:
      # Options: chesscom (Chess.com, default), tiantian (Tian Tian Xiangqi)
      sounds: chesscom
```

### Board and piece styles

```yaml
plugins:
  - search
  - chess:
      defaults:
        xiang:
          # Board options: default (classic), wood (wood grain), green (jade green)
          board: wood
          # Piece options: default (classic), ivory, ink (dark), wood (warm wood)
          pieces: ivory
        chess:
          # Board options: default (classic), wood (wood grain), green (green and cream)
          board: green
          # Piece options: default (classic), ivory (ivory and brown), ink (black and white)
          pieces: ivory
```

Override the site defaults for a single board with `board-<name>` and `pieces-<name>`.

```markdown
??? xiang board-green pieces-ink "rnbakabnr/9/1c5c1/p1p1p1p1p/9/9/P1P1P1P1P/1C5C1/9/RNBAKABNR w - - 0 1"
```

## Adding a game

The `BOARD_TYPES` registry in `markdown.py` defines each game's dimensions, FEN validation, and SVG rendering. Register a new type there and add a rules adapter with the same name in `board.mjs`. For example, a registered `shogun` type would use `??? shogun "POSITION"`. The current types are `chess` and `xiang`.

## Building the package

Run these commands from the repository root to build the source distribution and wheel, then check the package metadata.

```bash
python -m pip install build twine
python -m build
python -m twine check dist/*
```
