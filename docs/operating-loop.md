# Operating Loop

## 基本ループ

```text
ChatGPTで壁打ち
  -> 方針を詰める
  -> Codexへ会話を渡す
  -> GitHubを整理・実装
  -> GitHubに蓄積
  -> ChatGPTがGitHubを読む
  -> 設計レビュー・壁打ち
  -> Codexが更新
```

## 役割の境界

ChatGPT は設計会議の場。

Codex は GitHub 上に変更を入れる担当。

GitHub は昨日までの思考を固定する外部記憶。

Claude Code はローカル環境・ファイル・検証の担当。

## 1日の使い方

1. GitHub の README / docs / decisions を読む
2. 今日の対象タスクを1つ選ぶ
3. ChatGPT で方針を詰める
4. Codex に実装・整理を依頼する
5. GitHub に変更を残す
6. うまくいかなかった点を experiments に残す

## 改善対象

モデルの出力を感覚で評価しない。

次を改善対象にする。

- 情報の置き場所
- テンプレート
- 完了条件
- AGENTS.md
- Skill
- テスト
- 同期手順

