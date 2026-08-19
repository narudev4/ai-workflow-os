# GitHub to Sheets Sync

## 方針

GitHub を正本、Google Sheets を表示先として扱う。

ChatGPT や Codex が Sheets を直接編集する運用から始めるのではなく、GitHub の構造化データを Sheets に反映する運用から始める。

## 対応関係

```text
requirements.md  -> 要件概要シート
screens.csv      -> 画面一覧シート
features.csv     -> 機能一覧シート
schedule.csv     -> 工程表シート
questions.csv    -> 質問表シート
testcases.csv    -> テストケースシート
decisions/       -> 変更履歴・決定事項シート
```

## 同期原則

- GitHub 側のファイルを正とする
- Sheets 側の手編集は、確認後に GitHub へ戻す
- 未確定事項を requirements に混ぜない
- CSV は機械処理しやすい列名を維持する
- クライアント表示用の列名・書式は Sheets 側で整える

## 初期実装方針

最初は完全自動化しない。

1. GitHub の CSV を手動またはスクリプトで Sheets に貼る
2. 1案件で列設計の不足を確認する
3. 安定したら Google Sheets API / Apps Script / GitHub Actions で同期する

## 将来の同期フロー

```text
GitHub push
  -> GitHub Actions
  -> CSV/Markdown を読み込み
  -> Google Sheets API で更新
  -> 変更履歴を記録
```

## 注意

Sheets でクライアントが直接追記した内容は、すぐ正本とはみなさない。

人間が確認し、必要なら GitHub の requirements / questions / decisions に昇格する。

