# 機密情報取り扱いルール

## 保護対象
- docs/profile.md の内容（個人の弱点・キャリア目標）
- docs/minefield.md の内容（人物評価・社内政治）
- 具体的な社名・個人名が含まれるすべての情報

## 禁止事項
- 上記情報をコミットメッセージやPR descriptionに含めない
- 外部サービス（MCP経由含む）への送信時に個人名・社名を含めない
- decisions/ や logs/ に個人名を記載する場合はイニシャルまたはコードネームを使用

## Git管理
- profile.md と minefield.md は .gitignore で除外済み
- templates/ 配下のテンプレートのみGit管理する
- decisions/ と logs/ はGit管理するが、センシティブ情報は匿名化する
