# GitHub to Sheets Sync

## 方針

GitHub（Issues + Markdown）を正本、Google Sheets を先方とのインターフェースとして扱う。

先方はエンジニアではなく GitHub に馴染みがないため、先方が見る・書き込むのは Sheets のみ。MTG での状況共有・文字ベースのすり合わせ・質問への回答記入も Sheets 上で行う。GitHub 側は AI と Walkers の作業場に徹する。

## 対応関係

```text
Issues (label: task) + マイルストーン -> 工程表シート
Issues (label: question)             -> 質問表シート
requirements.md                      -> 要件概要シート
screens.md                           -> 画面一覧シート
features.md                          -> 機能一覧シート
testcases.md                         -> テストケースシート
decisions/                           -> 変更履歴・決定事項シート
```

## 同期原則

- GitHub 側を正とする
- Issue の状態（open/close、担当、期限、ラベル）をそのまま工程表・質問表の行にする
- Sheets 側の列名・書式は先方向けに整える（GitHub の用語をそのまま出さない）
- 先方が Sheets に書き込んだ内容は、すぐ正本とみなさない。人間が確認し、Issues / requirements / decisions に昇格する
- 未確定事項を requirements に混ぜない

## 初期実装方針

最初は完全自動化しない。

1. `gh issue list` と md を手動またはスクリプトで Sheets に反映する
2. 1案件で列設計・ラベル設計の不足を確認する
3. 安定したら GitHub Actions + Sheets API で同期する

## 将来の同期フロー

```text
Issue 更新 / GitHub push
  -> GitHub Actions
  -> Issues API + md を読み込み
  -> Google Sheets API で更新
  -> 変更履歴を記録
```
