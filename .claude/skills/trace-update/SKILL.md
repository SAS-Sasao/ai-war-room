---
name: trace-update
description: Claude Code会話履歴(~/.claude/projects/)を分析して、ユーザーの最新の作業傾向・思考パターンを抽出し、thinking-os.mdに反映する。Claude Code履歴は30日で削除されるため、定期的なアーカイブ役も兼ねる。
---

## 目的

Claude Code が ~/.claude/projects/ に保存している会話履歴（JSONL形式）を分析し、
ユーザーの直近の作業傾向・指示の癖・関心テーマを抽出する。
これにより、ai-war-room の decisions/logs だけでは見えない『日々の作業の生データ』が
thinking-os に反映される。

## 重要な前提

- Claude Code は ~/.claude/projects/ に会話履歴を保存している
- ファイル形式は JSONL（1行1JSON）
- パスのエンコード規則: スラッシュ→ハイフン
  - 例: /home/sasao020211/ai-war-room → -home-sasao020211-ai-war-room
- Claude Code はデフォルトで 30日経過したログを削除する
- そのため、定期実行でアーカイブも兼ねる必要がある

## 実行手順

### Step 1: 会話履歴の場所を特定

```bash
# 全プロジェクトの履歴ディレクトリを一覧
ls -la ~/.claude/projects/

# ai-war-room と cc-sier-organization の履歴に注目
ls -la ~/.claude/projects/-home-sasao020211-ai-war-room/ 2>/dev/null
ls -la ~/.claude/projects/-home-sasao020211-cc-sier-organization/ 2>/dev/null
```

### Step 2: アーカイブディレクトリの準備

ai-war-room/.archive/conversation-logs/ に分析済みログを保存する。
（このディレクトリは .gitignore で除外する）

```bash
mkdir -p .archive/conversation-logs/
```

### Step 3: JSONLファイルの読み込みと解析

各JSONLファイルから以下を抽出：

```bash
# Bashで実行する場合の例
for file in ~/.claude/projects/-home-sasao020211-ai-war-room/*.jsonl; do
  # ユーザー発言だけを抽出
  jq -r 'select(.type == "user") | .message.content' "$file" 2>/dev/null
done
```

ただしBashが使えない場合は、Read ツールで直接JSONLを読んでパースする。

### Step 4: ノイズ除外

以下は分析対象から除外：
- ツール承認の応答（「yes」「ok」だけのもの）
- 1行のみの短い指示
- システムメッセージ
- エラーリトライの繰り返し
- /clear や /resume などのスラッシュコマンド単独

### Step 5: 5つの観点で抽出

#### 5-1. 指示の癖
- どんな前置きで指示を始めるか
- 番号付きリスト vs 自然文 の比率
- 「！」「？」の使用頻度（熱量バロメーター）
- 「ultrathink」「丁寧に」「サクッと」等のモード指定の頻度

#### 5-2. 関心の集中領域
- どのトピックに何回言及しているか（頻度ランキング）
- 同じテーマが何セッションにまたがっているか
- 新しく登場したキーワードと、消えていったキーワード

#### 5-3. やり直し・修正パターン
- 「やっぱり」「修正して」「違う」の発生パターン
- AIの提案を一発で受け入れるか、何度も差し戻すか
- 完璧主義の発動条件（どんな品質ギャップで差し戻しが入るか）

#### 5-4. 委任 vs 自分でやる の比率
- 「やっておいて」と完全委任するパターン
- 「一緒に考えよう」と並走するパターン
- 「これは自分でやる」と切り分けるパターン
- それぞれの判断基準

#### 5-5. セッションの始まり方・終わり方
- 朝イチで何をするか
- 1セッションの典型的な長さ
- どんな状態でセッションを閉じるか（中途半端 / 完了 / 燃え尽き）

### Step 6: トレース結果の保存

抽出結果を以下の2箇所に保存：

#### 6-1. アーカイブとして保存
```
.archive/conversation-logs/trace-YYYY-MM-DD.md
```

フォーマット：
# 作業トレース YYYY-MM-DD

## 分析対象期間
{from} 〜 {to}

## 分析セッション数
- ai-war-room: N セッション
- cc-sier-organization: M セッション
- その他: K セッション

## 5つの観点

### 1. 指示の癖
{抽出結果}

### 2. 関心の集中領域
{頻度ランキングと変化}

### 3. やり直し・修正パターン
{パターン}

### 4. 委任 vs 自分でやる
{比率と判断基準}

### 5. セッションのリズム
{始まり方と終わり方}

## thinking-os.md への反映候補
- {追記すべき特徴1}
- {追記すべき特徴2}
- {更新すべき特徴}

#### 6-2. thinking-os.md への反映
trace-YYYY-MM-DD.md の「反映候補」セクションを元に、
thinking-os.md の該当セクションを更新する。

更新時のルール：
- 既存の記述を上書きしない（追記する）
- 新しい発見には「(YYYY-MM-DD のトレースから)」と注記
- base.md と矛盾する場合は「変化中」として両方記載

## 完了報告

実行完了後、以下を報告：
1. 分析したセッション数とプロジェクト
2. アーカイブしたファイルパス
3. thinking-os.md に追記した特徴の要約
4. 「30日経過したClaude Code履歴は削除されるので、月1での実行を推奨」と案内

## 注意事項

- ~/.claude/projects/ への読み取りは個人情報を含むので、外部送信しない
- アーカイブファイルは .gitignore で除外する
- 大量のセッションがある場合、最新7日分に絞ることも検討する
