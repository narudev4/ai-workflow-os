# Decision 0002: Sheetsをクライアント向けビューとして残す

## Status

Accepted

## Date

2026-08-19

## Decision

Google Sheets は廃止せず、クライアント共有用の表示UIとして残す。

## Reason

クライアントは従来どおり Sheets で画面一覧、機能一覧、工程表、質問表、テストケースを確認できる。GitHub に完全移行すると、先方確認の負荷が上がる可能性がある。

ただし、Sheets を正本にはしない。

## Consequences

- GitHub -> Sheets の同期設計が必要になる
- Sheets 側の直接編集は、確認後に GitHub へ戻す
- クライアント向けの見やすさは Sheets 側で担保する

