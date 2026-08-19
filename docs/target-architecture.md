# Target Architecture

## 全体像

```text
                       クライアント
                            |
              MTG / メール / チャット / モック
                            |
                            v
                    ChatGPT / Work
       要求整理・壁打ち・技術調査・レビュー・計画
                            |
                            v
                    GitHub Source of Truth
       requirements / screens / features / schedule
       questions / testcases / decisions / research
                            |
             +--------------+--------------+
             |                             |
             v                             v
       Google Sheets                 Codex / Claude Code
       共有・確認UI                   実装・検証・PR
             |
             v
        クライアント
```

## 役割分担

### ChatGPT

上流工程を担当する。

- 要求整理
- 壁打ち
- モック分析
- 技術調査
- 設計レビュー
- 意思決定支援

### GitHub

正本を担当する。

- 要件定義成果物
- 決定事項
- 調査結果
- テンプレート
- 変更履歴

### Google Sheets

クライアント向けの表示UIを担当する。

- 画面一覧
- 機能一覧
- 工程表
- 質問表
- テストケース

### Codex

GitHub上の実装・変更・レビューを担当する。

### Claude Code

ローカル環境での調査、検証、既存ファイル操作を担当する。

## 設計上のポイント

- AIを中心に置かない
- 案件の情報構造を中心に置く
- GitHubを記憶、Sheetsを表示、AIを工程別の実行者として扱う
- モデルが変わっても情報構造を維持できるようにする

