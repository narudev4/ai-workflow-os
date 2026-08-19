# Decision 0001: GitHubを正本にする

## Status

Accepted

## Date

2026-08-19

## Decision

案件・業務改善の正本を Google Sheets ではなく GitHub に置く。

## Reason

GitHub に Markdown / CSV として置くことで、ChatGPT、Codex、Claude Code が同じ情報を読める。変更履歴も残り、いつ・なぜ変わったかを追える。

Sheets を正本にすると、AIが扱うには構造が曖昧になりやすく、変更差分や意思決定の追跡も弱い。

## Consequences

- 要件定義成果物は GitHub に残す
- Sheets は GitHub から生成・同期する
- 未確定事項は questions に分離する
- 重要な判断は decisions に残す

