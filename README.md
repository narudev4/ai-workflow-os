# AI Workflow OS

AI Workflow OS は、Walkers の業務を AI が扱いやすい単位に構造化し、ChatGPT / GitHub / Google Sheets / Codex / Claude Code を役割分担させるための設計リポジトリです。

このリポジトリの初版は、2026-08-19 の「Codex改善ループ分析」チャットをもとに作成しています。

## 核心

```text
ChatGPT        上流の思考、要求整理、壁打ち、レビュー
GitHub         案件・業務改善の正本
Google Sheets  クライアント共有用の表示UI
Codex          GitHub上の実装、変更、レビュー
Claude Code    ローカル環境での調査、検証、開発
```

重要なのは、Google Sheets を捨てないことです。

ただし、Sheets は正本ではなく、GitHub にある構造化ファイルから生成・同期される表示先として扱います。AI が読む正本は GitHub に置き、人間・クライアントが見る成果物は Sheets に出します。

## 最初にやること

まず 1 案件だけで実験します。全社ディレクトリ整理、Hook、JSONL分析、Skill自動改善は後回しです。

1. 既存案件を 1 件選ぶ
2. 現行スプレッドシートを `templates/project/` の形に変換する
3. GitHub を正本として ChatGPT に読ませる
4. 要求整理・要件定義を ChatGPT で壁打ちする
5. 確定事項だけ GitHub に反映する
6. GitHub の CSV を Google Sheets に同期する
7. うまくいかなかった点を `experiments/` に記録する

最初の成功条件はこれだけです。

```text
現行スプシ
  -> GitHub正本
  -> ChatGPTで要求整理
  -> GitHub更新
  -> Sheets表示
  -> 先方確認
```

## リポジトリ構成

```text
README.md
AGENTS.md
docs/
  vision.md
  current-workflow.md
  target-architecture.md
  requirement-definition.md
  task-boundaries.md
  github-sheets-sync.md
  automation-roadmap.md
  operating-loop.md
  source-chat-summary.md
templates/
  project/
    README.md
    AGENTS.md
    requirements.md
    screens.csv
    features.csv
    schedule.csv
    questions.csv
    testcases.csv
    decisions/
      0000-template.md
    research/
      README.md
decisions/
  0001-github-as-source-of-truth.md
  0002-keep-sheets-as-client-view.md
  0003-start-with-one-project.md
  0004-defer-hooks-and-jsonl-analysis.md
experiments/
  2026-08-19-first-loop.md
```

## 運用原則

- 確定事項は GitHub に昇格する
- 未確定事項は requirements ではなく questions に置く
- 要件定義は詳細な API / DB / 実装設計に入る前で止める
- すべての作業単位に `Task`, `Input`, `Output`, `Done` を持たせる
- 意思決定は `decisions/` に日付・理由・出典つきで残す
- AI の失敗をすぐ AGENTS.md に追記しない。再発性があるものだけルール化する

## 次の入口

最初は [experiments/2026-08-19-first-loop.md](experiments/2026-08-19-first-loop.md) を開き、1案件で GitHub正本 -> Sheets表示 のループを回してください。

