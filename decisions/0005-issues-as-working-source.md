# Decision 0005: 動くものは Issues を正本にし、CSV を廃止する

## Status

Accepted

## Date

2026-08-27

## Decision

進行中に状態が動くもの（工程タスク・質問・先方確認事項）は GitHub Issues を正本とする。CSV ファイルは全廃する。画面一覧・機能一覧・テストケースなどの静的な一覧は Markdown の表で管理する。

Google Sheets は先方確認用のアウトプット専用とし、Issues と Markdown から生成する。

## Reason

- 既存案件で「工程表は Issues 正本 → スプシ同期」の運用実績があり、型が既に回っている
- 工程・質問は open/close、担当、期限、コメント履歴が本体であり、Issues の機能そのもの。CSV で持つと状態管理を自前で再発明することになる
- 先方はエンジニアではなく GitHub に馴染みがないため、先方が見るのは Sheets のみ。GitHub 側は AI と Walkers の作業場に徹する
- CSV は diff は取れるが、一覧性・編集性で md 表に対する優位が薄く、ファイル正本と Issues 正本の二重管理を生む

## Consequences

- `templates/project/` から全 CSV を削除する
- 工程タスク・質問は Issue テンプレート（`.github/ISSUE_TEMPLATE/`）から起票する
- 画面一覧・機能一覧・テストケースは `screens.md` / `features.md` / `testcases.md` の表で管理する
- Sheets 同期は「CSV → Sheets」から「Issues + md → Sheets」に変更する（docs/github-sheets-sync.md を改訂）
- Decision 0001 の「CSV として置く」という記述はこの決定で上書きされる
