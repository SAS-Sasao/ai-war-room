---
name: office-work
description: 事務作業の実行支援。Excel/PowerPoint/メール下書きなどの定型業務を代行・補助する。「資料作って」「Excel」「PPT」「スライド」「メール」等で起動。
---

## 対応可能な作業

### Excel
- データ整理・集計（csvやtsvからの変換含む）
- 関数・マクロの提案
- テンプレートからの報告書生成

### PowerPoint
- スライド作成・構成提案
- 既存スライドの修正

### メール（gws CLI経由）
- Gmail下書き作成: `gws gmail drafts create`
- メール検索・参照: `gws gmail messages list`
- 受信メールの要約

### Google Calendar（gws CLI経由）
- 予定の確認: `gws calendar events list`
- 予定の作成: `gws calendar events insert`

## gws CLI の使い方

Google Workspace CLIを使用する。未認証の場合は `gws auth setup` を案内する。
コマンド実行前にユーザーに確認を取る（特に送信・作成系）。

## 壁打ちからの引き継ぎ

`docs/decisions/` から直近のdecisionファイルが参照される場合、
そこに記載された「引き継ぎ > office-work」セクションの内容に基づいて作業する。
