# Decision 0004: HookとJSONL分析は後回しにする

## Status

Accepted

## Date

2026-08-19

## Decision

Hook、JSONLログ分析、AGENTS.md / Skill 自己改善は、最初の GitHub正本ループが成立してから着手する。

## Reason

タスク境界、成果物テンプレート、正本の場所が曖昧なままログ分析を始めると、改善対象がぼやける。AGENTS.md に一時的な失敗を積み上げると、ルールが肥大化して逆に性能が落ちる。

## Consequences

- まずは手動でタスク境界を記録する
- 再発する失敗だけを改善候補にする
- 改善先は AGENTS.md だけでなく、テンプレート、Skill、テストも候補にする

