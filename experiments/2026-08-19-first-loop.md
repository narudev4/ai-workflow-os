# Experiment: First GitHub Source of Truth Loop

## Date

2026-08-19

## Goal

既存案件1件で、GitHubを要件定義の正本、Google Sheetsをクライアント向けビューにする流れを1周させる。

## Scope

やること:

- 現行スプシの構造を確認する
- `templates/project/` を案件用にコピーする
- requirements / screens / features / schedule / questions / testcases に分解する
- ChatGPT に GitHub を読ませて要求整理を壁打ちする
- 確定事項だけ GitHub に反映する
- Sheets へ表示する
- 詰まった点を記録する

やらないこと:

- 全案件移行
- 完全自動同期
- Hook実装
- JSONL全量分析
- AGENTS.md自動更新

## Candidate Project

- Project:
- Current spreadsheet:
- Current repository:
- Owner:

## Steps

1. 対象案件を1つ選ぶ
2. 現行スプシのシート名と列を確認する
3. `templates/project/` との差分を確認する
4. GitHub正本に必要な最小列を決める
5. 現行データをCSV/Markdownへ変換する
6. ChatGPTで要求整理を実施する
7. 変更案を requirements / screens / features / questions / testcases に反映する
8. Sheetsへ同期または貼り付ける
9. 先方確認に耐えるか確認する
10. 実験ログを書く

## Success Criteria

- 1案件の要件定義成果物が GitHub に構造化されている
- Sheets 側でクライアントが見られる形になっている
- 未確定事項が questions に分離されている
- 少なくとも1つの意思決定が decisions に記録されている
- 次に改善すべきテンプレート上の不足が分かっている

## Log

### What worked

- 

### What failed

- 

### Missing columns / structure

- 

### Human decisions required

- 

### Next changes to this repository

- 

