# Project AGENTS.md

この案件リポジトリでは、GitHub を要件定義の正本として扱う。

## ルール

- 状態が動くもの（工程タスク・質問）は Issues が正本。ファイルに複製しない
- 確定事項だけ requirements.md / screens.md / features.md / testcases.md に反映する
- 未確定事項は question Issue として起票する。md に混ぜない
- 先方合意、社内判断、重要な方針変更は decisions/ に記録する
- Sheets は先方とのインターフェースであり、正本ではない。MTG でのすり合わせ・質問回答の記入に使い、書き込まれた内容は確認後に Issues / md に昇格する
- 要件定義では詳細なDB/API/実装設計に入らない

## 標準タスク

- requirement-analysis
- screens
- features
- schedule
- questions
- testcases
- client-review

各タスクでは `Task`, `Input`, `Output`, `Done` を明示する。task Issue のテンプレートがこの形になっている。
