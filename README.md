# AI作戦会議室 (ai-war-room)

Claude Codeを使った個人用AI作戦会議室。
業務の壁打ち・戦略コンサル・事務作業自動化を支援する。

## セットアップ

### 前提条件
- Claude Code CLI がインストール済み
- Node.js 18+ がインストール済み
- gh CLI がインストール済み（GitHub操作用）
- gws CLI がインストール済み（Google Workspace操作用）

### 初回セットアップ

1. リポジトリをクローン
2. プロファイルを作成:
   ```bash
   cd ai-war-room
   claude
   /profiler
   ```
3. 4つの質問に答えると `docs/profile.md` と `docs/minefield.md` が生成される

### MCP設定（オプション）

```bash
# Obsidian連携
claude mcp add --scope user obsidian -- npx @bitbonsai/mcpvault@latest /path/to/your/vault
```

## 日常の使い方

```bash
cd ai-war-room
claude
```

### 主なコマンド
| コマンド | 用途 |
|---------|------|
| `/kabeuchi {相談内容}` | 壁打ち・戦略コンサル |
| `/profiler` | プロファイル作成・更新 |
| `/daily-log` | 日報記録 |
| `/office-work {作業内容}` | 事務作業（Excel/PPT/メール） |
| `/nemawashi {相談内容}` | 根回し文案作成 |
| `/self-debate {案}` | 自分のアイデアを論破 |
| `/self-propose {テーマ}` | お前ならこうやる、と提案 |
| `/bias-check {文章}` | バイアスチェック |
| `/past-brainstorm {トレンド}` | 過去ログ×トレンドのブレスト |
| `/extract-thinking-os` | decisions+logs から思考OS抽出 |
| `/trace-update` | Claude Code履歴から作業傾向を抽出 |
| `/update-os [quick/full/check]` | 思考OS更新ラッパー |

## ディレクトリ構成

```
.claude/rules/       — 行動ルール（自動ロード）
.claude/skills/      — スキル定義（/コマンドで起動）
.claude/agents/      — サブエージェント定義
docs/profile.md      — 操縦者情報（.gitignore対象）
docs/minefield.md    — 地雷マップ（.gitignore対象）
docs/decisions/      — 壁打ち結果の蓄積
docs/logs/           — 日報ログ
docs/templates/      — テンプレート（Git管理）
```
