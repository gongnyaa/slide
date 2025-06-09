# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## コマンド

### スライド生成
```bash
npm run build
```
または
```bash
marp 20250611_Avoid_wasting_AI_effort --html --allow-local-files
```

このリポジトリはMarpを使用してMarkdownファイルからHTMLスライドを生成します。

### 自動化
Huskyのpre-commitフックにより、コミット時に自動的にmarpが実行され、生成されたHTMLファイルがコミットに含まれます。

## アーキテクチャ

このリポジトリは技術プレゼンテーション用のスライド管理システムです：

- **ファイル形式**: Marpの記法を使用したMarkdownファイル
- **出力**: HTMLスライド（GitHub Pagesで公開）
- **画像**: `image/` ディレクトリに格納
- **配信**: GitHub Pages経由（`https://gongnyaa.github.io/slide/`）

スライドファイルには以下のFrontMatter設定が必要：
```yaml
---
marp: true
theme: default
paginate: true
class: lead
---
```

新しいスライドを作成する際は、既存のファイル構造とスタイルを参考にしてください。