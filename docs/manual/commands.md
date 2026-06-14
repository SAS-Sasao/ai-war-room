# AI作戦会議室 コマンドリファレンス

最終更新: 2026-04-10
対象バージョン: 思考OSシステム v1(shadow-self 統合版)

## このドキュメントについて

本ドキュメントは AI作戦会議室(ai-war-room)で利用可能な全スラッシュコマンドの機能・実行状況・詳細手順を記載するリファレンスです。日々の運用で「どのコマンドを、いつ、どう使うか」を迷ったらここに戻ってください。

---

## 全体像

コマンドは大きく3層に分かれます：

```
┌──────────────────────────────────────────┐
│ 第3層: 分身(shadow-self)系                │  ← 複利で育つ
│ /self-debate /self-propose                │
│ /bias-check  /past-brainstorm             │
├──────────────────────────────────────────┤
│ 第2層: 思考OS系(蓄積エンジン)              │  ← システムの心臓
│ /extract-thinking-os /trace-update        │
│ /update-os                                │
├──────────────────────────────────────────┤
│ 第1層: 基礎スキル(即実行可能)              │  ← 日常業務
│ /profiler  /daily-log  /kabeuchi          │
│ /office-work  /nemawashi                  │
└──────────────────────────────────────────┘
```

**第1層** が日常業務を支え、**第2層** がそこから思考OSを抽出し、**第3層** の分身が思考OSを参照して本人並みの判断を下す、という重層構造です。

---

## 前提条件

すべてのコマンドに共通する前提：

1. Claude Code が起動している(`claude` コマンド)
2. カレントディレクトリが `ai-war-room/` であること
3. 初回は `/profiler` で `docs/profile.md` と `docs/minefield.md` を生成済みであること

---

## 使用状況の凡例

- ✅ **即使える**: 単独で動く
- ⚠️ **使えるが複利で育つ**: 動くが、蓄積が薄い現時点では出力品質が限定的
- ⏳ **準備必要**: 先に別のコマンドを実行する必要あり

---

## コマンド一覧(サマリー表)

| コマンド | カテゴリ | 状況 | 一行用途 |
|---------|---------|------|---------|
| `/profiler` | 基礎 | ✅ | プロファイル作成・更新 |
| `/daily-log` | 基礎 | ✅ | 日報記録 |
| `/kabeuchi` | 基礎 | ✅ | 壁打ち・戦略コンサル |
| `/office-work` | 基礎 | ✅ | 事務作業(Excel/PPT/メール) |
| `/nemawashi` | 基礎 | ✅ | 根回し文案作成 |
| `/extract-thinking-os` | 思考OS | ⚠️ | decisions/logs から思考OS抽出 |
| `/trace-update` | 思考OS | ⚠️ | Claude Code履歴から作業傾向抽出 |
| `/update-os` | 思考OS | ⚠️ | 思考OS更新ラッパー |
| `/self-debate` | 分身 | ⚠️ | 自分のアイデアを論破 |
| `/self-propose` | 分身 | ⚠️ | 能動的に提案させる |
| `/bias-check` | 分身 | ⚠️ | 成果物のバイアス監査 |
| `/past-brainstorm` | 分身 | ⚠️ | 過去 × トレンドで新結合 |

**現時点(2026-04-10)の補足**:
- 第1層(基礎スキル)はすべて即使えます
- 第2層・第3層は動きますが、`thinking-os.md` がまだ未生成 & trace archive が空なので2層プロファイル(base.md + profile.md)での簡易モードです
- `/update-os full` を1回走らせると第2層・第3層の品質が跳ね上がります

---

## 第1層: 基礎スキル

### /profiler

**機能**: `docs/profile.md`(現在地・目的地・操縦者のバグ)と `docs/minefield.md`(地雷マップ)を作成・更新する。

**使用状況**: ✅ 即使える

**起動方法**:
- `/profiler` または `/profiler reset` → フルヒアリング(ゼロから作成)
- `/profiler update` → 差分更新
- `/profiler update {内容}` → ピンポイント更新

**実行フロー**:
1. 既存の profile.md / minefield.md を確認
2. reset モード → 4つの質問を順番に投げる(現在地 / 目的地 / 地雷 / バグ)
3. update モード → 変更ありそうな部分を1つずつ確認
4. 回答を所定のフォーマットで保存

**出力**: `docs/profile.md` / `docs/minefield.md`

**使い所**:
- 初回セットアップ時
- 役職・権限・立場が変わった時
- 地雷マップに大きな変動(上司異動等)があった時

---

### /daily-log

**機能**: その日やったこと・考えたこと・明日やることを構造化して記録する。

**使用状況**: ✅ 即使える

**起動方法**: `/daily-log`

**出力**: `docs/logs/YYYY-MM-DD.md`

**使い所**:
- 1日の締めに
- 週次振り返り前のウォームアップに

---

### /kabeuchi

**機能**: 業務の壁打ち・ブレスト・戦略コンサルティング。3フェーズ構成で方針決定まで導く。

**使用状況**: ✅ 即使える

**起動方法**: `/kabeuchi {相談内容}` または `/kabeuchi` のみ

**実行フロー**:
1. profile.md / minefield.md / 直近 decisions を読む
2. フェーズ1: 状況整理(「つまりこういうこと?」)
3. フェーズ2: 選択肢提示(2〜3案)、各案にメリット/リスク/根回しを整理
4. フェーズ3: 方針決定 + アクションプラン
5. 結論を `docs/decisions/YYYY-MM-DD-{topic}.md` に保存

**出力**: `docs/decisions/YYYY-MM-DD-{topic}.md`

**使い所**:
- 方針に迷った時
- 根回しルートを設計したい時
- タスクの優先度を判断したい時

**連携**:
- 結論から → `/nemawashi` でメール文案生成
- 結論から → `/office-work` で資料化
- 結論の検証 → `/self-debate` で反対側から論破

---

### /office-work

**機能**: 事務作業(Excel/PPT/メール下書き等)の実行支援。

**使用状況**: ✅ 即使える

**起動方法**: `/office-work {作業内容}`

**例**:
- `/office-work 面談用の1枚サマリーをPPTで作って`
- `/office-work 週報のExcel集計`

**使い所**:
- ルーチン業務の高速化
- テンプレ流用で Lv.1 対応したい時

---

### /nemawashi

**機能**: 根回しメール・Slack・Teams 文案の作成。

**使用状況**: ✅ 即使える

**起動方法**: `/nemawashi {相談内容}`

**実行フロー**:
1. minefield.md で宛先の地雷を確認
2. 目的とトーンを整理
3. 角が立たない文案を数パターン提示
4. 選択 → 最終文案

**使い所**:
- 社内調整・依頼・報告・お断り
- 初めて接点を持つ相手へのメール

---

## 第2層: 思考OS系

### /extract-thinking-os

**機能**: `docs/decisions/` と `docs/logs/` から最近の意思決定トレンドを抽出し、`.claude/rules/thinking-os.md` を生成する。`thinking-os-base.md` と照合して差分(一貫/変化/新規発見)を記録する。

**使用状況**: ⚠️ 使えるが複利で育つ(decisions/logs が厚いほど精度↑)

**起動方法**: `/extract-thinking-os`

**実行フロー**:
1. `docs/decisions/` 直近10件 + `docs/logs/` 直近5件を読む
2. 5項目でパターン抽出(関心テーマ / 判断基準 / 手詰まり / コミュニケーション / 学習)
3. `thinking-os-base.md` と照合(一貫 / 変化 / 新規発見 / フェーズ判定)
4. `.claude/rules/thinking-os.md` を生成

**出力**: `.claude/rules/thinking-os.md`

**前提**:
- `.claude/rules/thinking-os-base.md` が存在すること(既に作成済み)
- decisions / logs に最低数件の蓄積があること

**使い所**:
- 月1の思考OS棚卸し
- `/update-os quick` 内部から呼ばれる

---

### /trace-update

**機能**: `~/.claude/projects/` の Claude Code 会話履歴(JSONL)を分析し、作業傾向・指示の癖・関心テーマを抽出して thinking-os.md に反映する。アーカイブも兼ねる。

**使用状況**: ⚠️ 使えるが複利で育つ

**起動方法**: `/trace-update`

**実行フロー**:
1. `~/.claude/projects/` の該当プロジェクトディレクトリを特定
2. `.archive/conversation-logs/` を作成(.gitignore 対象)
3. JSONL ファイルからユーザー発言を抽出
4. ノイズ除外(yes/ok、スラッシュコマンド単独等)
5. 5観点で抽出(指示の癖 / 関心集中 / 修正パターン / 委任比率 / セッションリズム)
6. `.archive/conversation-logs/trace-YYYY-MM-DD.md` に保存
7. `thinking-os.md` に追記反映

**重要な前提**: Claude Code の会話履歴はデフォルトで30日経過後に削除されます。そのため、月1での実行を強く推奨します。

**出力**:
- `.archive/conversation-logs/trace-YYYY-MM-DD.md`
- `.claude/rules/thinking-os.md` への追記

**使い所**:
- 月1の定期アーカイブ
- `/update-os full` 内部から呼ばれる

---

### /update-os

**機能**: 思考OSの更新ラッパー。内部で複数のスキルを順次呼び出す。

**使用状況**: ⚠️ 使えるが複利で育つ

**起動方法**:
- `/update-os` または `/update-os quick` → クイック更新(decisions/logs のみ)
- `/update-os full` → フル更新(trace-update も含む・最推奨)
- `/update-os check` → 差分プレビューのみ(ドライラン)

**実行フロー(full モード)**:
1. `/trace-update` を実行
2. `/extract-thinking-os` を実行
3. 完了レポート出力(分析件数 / 変更点 / 整合性チェック / 次のアクション提案)

**使い所**:
- 月1のメンテナンス
- 分身系スキル(self-debate 等)を使う前のリフレッシュ
- 大きな業務変化の後

**推奨タイミング**: 毎月1日・月末、または立場・役割が変わった直後

---

## 第3層: 分身(shadow-self)系

4つすべてが `.claude/agents/shadow-self.md` エージェントを内部で起動し、3層プロファイル(base.md / thinking-os.md / profile.md)を縦串で引用します。

### /self-debate

**機能**: ユーザーが出した方針・決断・提案に対して、本人の過去発言・失敗パターン・価値観を引いて容赦なく論破する。

**使用状況**: ⚠️ 使えるが複利で育つ

**起動方法**: `/self-debate {お題}` または `/self-debate` のみ

**実行フロー**:
1. shadow-self の必須コンテキスト読み込み(6ファイル)
2. お題を「主張/前提/期待結果/想定リスク」に分解
3. 4パターンで論破:
   - profile ⇔ base.md の矛盾突き
   - base.md ⇔ 最近 decisions の一貫性チェック
   - minefield との衝突
   - 過去の失敗パターンの再発検知
4. 締めは固定トリガー「で、本当はどうしたいの?」

**使い所**:
- 自分で決めた方針の最終チェック
- 行動直前の自己論破
- 大事な意思決定の「悪魔の代弁者」

**注意**: 同意・励まし禁止。優しくはない。

---

### /self-propose

**機能**: ユーザーが行き詰まっている or 案が無いテーマに対して、本人の思考OSで「俺ならこうやる」と3案提示する。

**使用状況**: ⚠️ 使えるが複利で育つ

**起動方法**: `/self-propose {テーマ}` または引数なし

**実行フロー**:
1. shadow-self の必須コンテキスト読み込み
2. base.md の「思考の5つの柱」を活かして3案生成
3. 各案に「根拠(過去の証跡) / 刺さる理由 / 最初の一歩」を付与
4. 最後に「俺の本命」で1案を推す

**使い所**:
- アイデアが枯渇した時
- 視点を変えたい時
- 上司への提案の初期案が欲しい時

**self-debate との違い**: self-debate は「既にある案の論破」、self-propose は「ゼロからの発想」。

---

### /bias-check

**機能**: ユーザーが作成した成果物(メール・スライド・提案等)を、本人固有のバイアスで監査する。

**使用状況**: ⚠️ 使えるが複利で育つ

**起動方法**:
- `/bias-check {成果物テキスト}`
- `/bias-check {ファイルパス}`
- `/bias-check` (対話モード)

**実行フロー**:
1. shadow-self の必須コンテキスト読み込み
2. 宛先・用途・重要度(Lv.1〜3)を判定
3. 6項目で該当分だけ指摘:
   - ① 熱量バロメーター暴走
   - ② 完璧主義暴走
   - ③ 一気通貫思考暴走
   - ④ 仕組み先行の押し付け
   - ⑤ minefield との衝突
   - ⑥ 過去 decisions との矛盾
4. 「引用ソース → 問題 → 修正案」の3段で出力

**使い所**:
- メール送信直前
- 資料提出前
- 上司・クライアント向けの発信前

---

### /past-brainstorm

**機能**: 外部トレンドを受け取り、ユーザーの過去資産(decisions / logs / trace archives)と突き合わせて新結合を起こす。思考OSシステムで唯一「外向き × 時間軸」を扱うスキル。

**使用状況**: ⚠️ 使えるが複利で育つ(特にこのスキルは複利効果大)

**起動方法**:
- `/past-brainstorm {トレンド}`
- `/past-brainstorm @file:path/to/article.md`
- `/past-brainstorm` (対話モード)

**実行フロー**:
1. shadow-self の必須コンテキスト読み込み
2. トレンドから検索キーワード抽出(中心/周辺/構造)
3. decisions / logs / `.archive/conversation-logs/` を Grep 横断
4. 上位5〜8件を Read で全文読み込み
5. 5観点で新結合を発掘:
   - ① 構造の類似性
   - ② 学びの連続性
   - ③ 人脈・地雷ルートの再利用
   - ④ 失敗の転用
   - ⑤ 未完了の再起動 ← 最重要
6. 新結合3つ + 「俺の本命」を出力

**使い所**:
- 業界記事を読んだ直後
- 新しいトレンドワードに触れた時
- 過去の「やりたかったけど流れた」構想の復活チャンスを探したい時

**複利のコツ**: `/update-os full` で trace archive を育てるほど、観点5(未完了の再起動)の精度が跳ね上がります。

---

## 推奨運用フロー

### 日次フロー(5分)

```
朝: /kabeuchi で今日の優先順位を整理
    ↓
昼: 個別タスク実行(/office-work, /nemawashi など)
    ↓
夜: /daily-log で1日を記録
```

### 週次フロー(15分)

```
週末: /kabeuchi で週次振り返り
      ↓
      (必要に応じて) /profiler update で差分更新
```

### 月次フロー(30分)

```
月末 or 月初: /update-os full
              ↓
              生成された thinking-os.md を確認
              ↓
              /self-debate で今月の大きな意思決定を再検証
              ↓
              (気になるトレンドがあれば) /past-brainstorm
```

### イベントドリブン

- **方針に迷った時**: `/kabeuchi`
- **既にある案が不安**: `/self-debate`
- **案が出ない**: `/self-propose`
- **提出前の最終チェック**: `/bias-check`
- **新しいトレンドを見た時**: `/past-brainstorm`
- **立場が変わった時**: `/profiler update`

---

## 初回セットアップ手順(このシステムを初めて使う場合)

1. `/profiler` → 4つの質問に答えて profile.md / minefield.md を生成
2. 1週間くらい `/kabeuchi` と `/daily-log` を普通に使う
3. decisions が5件以上貯まったら `/update-os full` を初回実行
4. `thinking-os.md` が生成されたら `/self-debate` を試してみる
5. 月次運用に移行

この順番を飛ばして第3層から始めると、分身の引用ソースが薄くて満足のいく出力にならない可能性があります。

---

## ファイル配置マップ

```
ai-war-room/
├── CLAUDE.md                    ← プロジェクト指示(自動ロード)
├── README.md                    ← 概要
├── .claude/
│   ├── rules/                   ← 行動ルール(自動ロード)
│   │   ├── communication.md
│   │   ├── consulting-style.md
│   │   ├── safety.md
│   │   ├── task-routing.md
│   │   ├── thinking-os-base.md  ← 基底プロファイル(半年〜年1回手動更新)
│   │   └── thinking-os.md       ← 最近の判断トレンド(自動生成・未生成)
│   ├── skills/                  ← スキル定義(/コマンド)
│   │   ├── profiler/
│   │   ├── daily-log/
│   │   ├── kabeuchi/
│   │   ├── office-work/
│   │   ├── nemawashi/
│   │   ├── extract-thinking-os/
│   │   ├── trace-update/
│   │   ├── update-os/
│   │   ├── self-debate/
│   │   ├── self-propose/
│   │   ├── bias-check/
│   │   └── past-brainstorm/
│   └── agents/                  ← サブエージェント定義
│       ├── consultant.md
│       ├── daily-reporter.md
│       ├── nemawashi-writer.md
│       ├── office-worker.md
│       └── shadow-self.md       ← 分身エージェント
├── docs/
│   ├── profile.md               ← 操縦者情報(.gitignore)
│   ├── minefield.md             ← 地雷マップ(.gitignore)
│   ├── decisions/               ← 壁打ち結果の蓄積
│   ├── logs/                    ← 日報
│   ├── templates/               ← テンプレート
│   └── manual/
│       └── commands.md          ← このファイル
└── .archive/                    ← trace archive (.gitignore)
    └── conversation-logs/
```

---

## トラブルシューティング

### Q. `/self-debate` を叩いたのに引用ソースが薄い
**A**: `thinking-os.md` が未生成 or decisions の蓄積が少ないため。`/update-os full` を実行してください。

### Q. `/past-brainstorm` の出力が「接点が薄い」と言われる
**A**: trace archive が空の可能性。`/update-os full` で trace archive を生成してから再実行してください。

### Q. `/trace-update` で `~/.claude/projects/` が見つからない
**A**: パスのエンコード規則を確認してください。`/home/user/ai-war-room` → `-home-user-ai-war-room` のようにスラッシュがハイフンに変換されます。

### Q. commit したくないファイルまで git status に出てくる
**A**: `.gitignore` を確認してください。profile.md / minefield.md / .archive/ / .claude/settings.local.json は除外対象です。

### Q. profile.md / minefield.md が読み込まれない
**A**: `docs/profile.md` と `docs/minefield.md` が存在することを確認してください。なければ `/profiler` で生成できます。

### Q. Claude Code 履歴の保持期間を延ばしたい
**A**: `~/.claude/settings.json` の `cleanupPeriodDays` で設定可能。デフォルトは30日。

---

## スキル間の連携マップ

```
                    ┌────────────┐
                    │  /profiler │  (初期化)
                    └──────┬─────┘
                           ↓
      ┌────────────────────┼────────────────────┐
      ↓                    ↓                    ↓
 ┌─────────┐        ┌──────────┐         ┌──────────┐
 │/kabeuchi│───────→│/nemawashi│         │/office-  │
 │         │        └──────────┘         │  work    │
 └────┬────┘                             └──────────┘
      ↓                                        ↑
 ┌─────────┐                                   │
 │ decisions│                                  │
 └────┬─────┘                                  │
      ↓                                        │
 ┌──────────────┐    ┌──────────────┐          │
 │/extract-     │←───│/update-os    │          │
 │thinking-os   │    │   full       │          │
 └──────┬───────┘    └──────┬───────┘          │
        ↓                   ↓                  │
  ┌──────────┐       ┌─────────────┐           │
  │thinking- │       │/trace-update│           │
  │os.md     │       └─────────────┘           │
  └────┬─────┘                                 │
       ↓                                       │
 ┌─────────────────────────────────────┐       │
 │  shadow-self(3層プロファイル参照)    │       │
 └────┬────────────┬────────────┬──────┘       │
      ↓            ↓            ↓              │
┌───────────┐┌──────────┐┌────────────┐┌──────────────┐
│/self-     ││/self-    ││/bias-check ││/past-        │
│  debate   ││ propose  ││            ││ brainstorm   │
└───────────┘└──────────┘└────────────┘└──────────────┘
                                             ↑
                                   ┌─────────┴──────────┐
                                   │ 外部トレンド入力     │
                                   └────────────────────┘
```

---

## 今後の拡張候補

- `/case-bank`: 過去の成功パターンをタグ付けして再利用可能な Case として蓄積
- `/monthly-review`: 月次で decisions / logs から自動サマリー生成
- `/figma-import`: Figma MCP と連携してデザイン資料を取り込む
- `/trend-watch`: RSS/はてブ等からトレンドを自動取得して `/past-brainstorm` へ自動投入

---

*本ドキュメントは思考OSシステムの成熟に合わせて随時更新される想定です。*
