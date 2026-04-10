---
name: update-os
description: 思考OSの更新ラッパー。decisions/logsとClaude Code会話履歴の両方から思考OSを更新する。「OS更新」「思考更新」「分身更新」で起動。
---

## 目的

思考OSの更新を1コマンドで完結させるラッパー。
内部で複数のスキルを順次呼び出す。

## 呼び出しモード

### `/update-os` または `/update-os quick` → クイック更新
- decisions と logs だけから更新（数秒で完了）
- 内部的に /extract-thinking-os を実行

### `/update-os full` → フル更新
- decisions + logs + Claude Code会話履歴 を全部分析
- 内部的に /trace-update → /extract-thinking-os の順で実行
- 数分かかるが最も精度が高い

### `/update-os check` → 差分確認のみ
- 現在の thinking-os.md と新しく抽出される内容の差分だけ表示
- 実際のファイル更新は行わない（プレビューモード）

## 実行手順

### quick モード
1. /extract-thinking-os を実行
2. 完了後、変更点のサマリーを3行で報告
3. 「もっと深く分析したければ /update-os full を使ってね」と案内

### full モード
1. まず /trace-update を実行（Claude Code会話履歴の分析）
2. 次に /extract-thinking-os を実行（base.md と照合した抽出）
3. 完了後、以下を報告：
   - 分析した会話セッション数
   - 抽出した新しい特徴
   - profile.md / base.md / thinking-os.md の3層の整合性チェック結果

### check モード
1. /extract-thinking-os の処理を「ドライラン」で実行
2. 現在のthinking-os.mdとの差分を計算
3. 差分のみ表示（ファイル書き込みなし）
4. ユーザーが「OK」と言ったら本実行

## 完了時のレポートフォーマット

# 思考OS更新レポート

更新日時: YYYY-MM-DD HH:MM
更新モード: quick / full / check
分析対象:
- decisions: N件
- logs: M件
- Claude Code会話履歴: P セッション（fullモードのみ）

## 主な変更点
1. {変更1}
2. {変更2}
3. {変更3}

## profile.md との整合性
- 一致: {項目}
- ズレ: {項目}
- 自己申告にない発見: {項目}

## 次のアクション提案
- {分身が提案する次の壁打ちテーマや確認事項}
