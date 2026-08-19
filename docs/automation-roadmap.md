# Automation Roadmap

## 優先順位

```text
1. ディレクトリ・コンテキスト整理
2. 案件単位の正本設計
3. GitHub -> Sheets 表示
4. 要件定義のタスク境界と完了条件
5. ChatGPT で上流工程
6. Codex / Claude Code で実装
7. MTG 自動化
8. Hook
9. JSONL ログ分析
10. Skill / AGENTS.md 自己改善
```

## 後回しにするもの

最初から次を作らない。

- AGENTS.md 自動更新
- 毎タスクのJSONL全量分析
- 完全自動のタスク境界判定
- 全案件の一括移行
- Sheets完全同期

## 先にやる理由

Hook や自己改善は、正本の場所、タスク境界、成果物テンプレートが固まってからでないと、改善対象が曖昧になります。

まず 1 案件で次を確認します。

- GitHub 正本にすべき情報は何か
- Sheets に見せるべき列は何か
- ChatGPT が詰めるべき工程はどこか
- Codex / Claude Code に渡すべき成果物は何か
- 人間判断が必要な地点はどこか

## MTG自動化の位置づけ

MTG 自動化は要件定義正本とは別の Automation Pipeline として扱う。

```text
tl;dv
  -> Webhook
  -> Meeting Processor
  -> meeting.md
  -> decisions/
  -> questions.csv
  -> requirements 変更候補
  -> 人間レビュー
  -> GitHub反映
```

議事録を保存するだけでなく、要求変更、影響範囲、質問、Next Action に分解する。

