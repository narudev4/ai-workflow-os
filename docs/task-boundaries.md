# Task Boundaries

## 考え方

長い AI セッション自体は問題ではありません。

問題は、セッション内で何の成果物を作っているのかが曖昧になることです。

そのため、セッションではなく成果物単位でタスク境界を作ります。

## タスク定義フォーマット

```text
Task:
Input:
Output:
Done:
```

## 例

```text
Task: 質問表の作成
Input:
- requirements.md
- screens.csv
- features.csv
- schedule.csv
Output:
- questions.csv
Done:
- 未確定事項が重複なく並んでいる
- 先方に聞く理由が分かる
- 回答後にどの成果物へ反映するか分かる
```

## 初期運用

最初からタスク境界を完全自動判定しない。

まずは人間が明示します。

```text
/task-start requirement-analysis
...
/task-complete requirement-analysis
```

この程度の明示でも、ログ分析や改善ループの精度は大きく上がります。

## 将来の自動判定候補

- ユーザーが新しい目的を提示した
- 別案件・別Issueに移った
- コミットした
- PRを作った
- テストが通った
- Codexが完了報告した
- 人間が成果物を承認した

ただし、自動判定は後回しです。まずは人間が境界を記録する運用を成立させます。

