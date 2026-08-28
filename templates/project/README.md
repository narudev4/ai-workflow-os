# Project Requirements Repository

このディレクトリは、1案件を GitHub 正本として管理するためのテンプレートです。新規案件が始まったら、これを private リポジトリにコピーして開始します。

## 正本の置き場所

| 種類 | 正本 | 理由 |
|---|---|---|
| 工程タスク | Issues（label: `task`） | 状態・担当・期限・履歴が動くため |
| 質問・未確定事項 | Issues（label: `question`） | open/close で管理し、回答が出たら md / decisions に昇格する |
| 要求・要件 | `requirements.md` | 確定事項のみ |
| 画面一覧 | `screens.md` | 確定事項のみ |
| 機能一覧 | `features.md` | 確定事項のみ |
| テストケース | `testcases.md` | 確定事項のみ |
| 意思決定 | `decisions/` | 日付・理由・出典つき |
| 調査資料 | `research/` | 参照資料 |

Google Sheets は先方とのインターフェース。先方は GitHub を見ないため、先方に見せるものはすべて Sheets に出し、MTG での状況共有・文字ベースのすり合わせ・質問への回答記入も Sheets 上で行う。先方が書き込んだ内容は確認後に Issues / md へ昇格する（正本は GitHub のまま）。

## Issues の運用

- 起票は `.github/ISSUE_TEMPLATE/` のテンプレートから行う
- マイルストーン = フェーズ（要件定義、開発、リリース、研修など）
- ラベル: `task` / `question` / `client-review`（先方確認待ち）/ `blocked`（依存で止まっている）
- close の条件: task は Done を満たしたとき、question は回答が md か decisions に昇格されたとき

## 使い方

1. このテンプレートを案件用 private リポジトリにコピーする
2. requirements.md のプロジェクト情報を埋める
3. 工程をマイルストーンと task Issue に展開する
4. 未確定事項を question Issue として起票する
5. 確定したものだけ md / decisions に昇格する
6. Issues と md から先方確認用スプレッドシートを生成・更新する

## 注意

このテンプレートは要件定義用です。詳細設計や実装コードは、合意後に別工程として扱います。
