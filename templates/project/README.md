# Project Requirements Repository

このディレクトリは、1案件を GitHub 正本として管理するためのテンプレートです。

## 使い方

1. このテンプレートを案件用リポジトリにコピーする
2. 現行スプレッドシートの内容を CSV / Markdown に変換する
3. 未確定事項は `questions.csv` に置く
4. 決定事項は `decisions/` に記録する
5. Sheets はこのリポジトリから生成・同期する

## 標準ファイル

- `requirements.md`: 要求・要件の概要
- `screens.csv`: 画面一覧
- `features.csv`: 機能一覧
- `schedule.csv`: 工程表
- `questions.csv`: 質問表
- `testcases.csv`: テストケース
- `decisions/`: 意思決定ログ
- `research/`: 技術調査・参照資料

## 注意

このテンプレートは要件定義用です。詳細設計や実装コードは、合意後に別工程として扱います。

