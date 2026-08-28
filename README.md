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

ただし、Sheets は正本ではなく、GitHub の Issues と Markdown から生成・同期される先方とのインターフェースとして扱います。MTG での状況共有・文字ベースのすり合わせ・質問への回答記入は Sheets 上で行い、書き込まれた内容は確認後に GitHub へ昇格します。AI が読む正本は GitHub に置き、クライアントが見る成果物は Sheets に出します。クライアントはエンジニアではなく GitHub には馴染みがないため、GitHub を直接見せることはしません。

正本の分担は Issue 駆動です。状態が動くもの（工程タスク・質問）は Issues、確定した一覧（要件・画面・機能・テストケース）は Markdown、意思決定は decisions/ に置きます。CSV は使いません（Decision 0005）。

## 最初にやること

まず 1 案件だけで実験します。全社ディレクトリ整理、Hook、JSONL分析、Skill自動改善は後回しです。

1. 既存案件を 1 件選ぶ
2. `templates/project/` を案件用 private リポジトリにコピーする
3. 現行スプレッドシートの工程・未確定事項を Issues に、確定事項を Markdown に変換する
4. GitHub を正本として ChatGPT に読ませ、要求整理・要件定義を壁打ちする
5. 確定事項だけ GitHub に反映する
6. Issues と Markdown から先方確認用の Google Sheets を生成する
7. うまくいかなかった点を `experiments/` に記録する

最初の成功条件はこれだけです。

```text
現行スプシ
  -> GitHub正本（Issues + md）
  -> ChatGPTで要求整理
  -> GitHub更新
  -> Sheets出力
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
  claude-session-2026-08-17-loop-engineering.md
templates/
  project/
    README.md
    AGENTS.md
    requirements.md
    screens.md
    features.md
    testcases.md
    .github/
      ISSUE_TEMPLATE/
        01-task.md
        02-question.md
    decisions/
      0000-template.md
    research/
      README.md
decisions/
  0001-github-as-source-of-truth.md
  0002-keep-sheets-as-client-view.md
  0003-start-with-one-project.md
  0004-defer-hooks-and-jsonl-analysis.md
  0005-issues-as-working-source.md
experiments/
  2026-08-19-first-loop.md
```

## 運用原則

- 確定事項は GitHub に昇格する
- 未確定事項は requirements ではなく question Issue に置く
- 工程は task Issue + マイルストーンで管理する。工程表ファイルは作らない
- 要件定義は詳細な API / DB / 実装設計に入る前で止める
- すべての作業単位に `Task`, `Input`, `Output`, `Done` を持たせる
- 意思決定は `decisions/` に日付・理由・出典つきで残す
- AI の失敗をすぐ AGENTS.md に追記しない。再発性があるものだけルール化する

## 次の入口

Claude Codeで先に検証されたMTG後処理ループから壁打ちを再開する場合は、[docs/claude-session-2026-08-17-loop-engineering.md](docs/claude-session-2026-08-17-loop-engineering.md) を開いてください。

GitHub正本 -> Sheets表示の最初の実験は、[experiments/2026-08-19-first-loop.md](experiments/2026-08-19-first-loop.md) を入口にしてください。
