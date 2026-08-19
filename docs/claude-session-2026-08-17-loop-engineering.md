# Claudeセッション引き継ぎ: WalkersのLoop Engineering

## この文書の目的

Claude Codeで進めていたWalkersの業務ループ設計を、ChatGPTとの壁打ちに再接続するためのブリーフです。Claudeの生ログはリポジトリにコピーせず、設計上必要な事実・判断・未解決事項だけを要約しています。

## 出典

- Claude session ID: `12e36a69-69e0-4b45-861b-593504c3e3a3`
- 作業ディレクトリ: `/Users/naru/Walkers_naru`
- セッション日: 2026-08-17
- 関連する上流チャット: `Codex改善ループ分析` (`6a847ceb-28c4-83e9-a3c4-f84fae52aaff`)

## 先に結論

このセッションで、Walkersの「Loop Engineering」は、常駐チャットを人間が維持する方式から、業務イベントを検知して必要なAI処理を無人実行する方式へ一段進んだ。

実証できた最小ループは次の通り。

```text
tl;dvにMTG transcriptが生成される
  -> watcherが5分間隔で検知
  -> claude -p が対象MTGの処理を起動
  -> workerが議事録生成・NA登録・必要ならメール下書きを作成
  -> macOS通知で完了を知らせる
  -> naruが送信・取消などの判断だけ行う
```

ただし、これは「Walkers全体のLoop Engineeringが完成した」という意味ではない。2026-08-17時点で実弾検証できたのは、MTG後処理ループの1本である。Windows/Macをまたぐ依頼インボックス、mission-controlからのGO、案件横断の状態管理は、次の設計課題として残っている。

## 会話の流れ

### 1. html-shareから出発

最初の問いは、`minorun365/html-share` のような「AIが生成したHTMLを別端末から見る」仕組みを、WalkersのWindows/Mac運用に活かせるかだった。

ここで見えた本質は、HTML共有そのものではなく次の仕事の流れだった。

```text
外出先のスマホで依頼をストック
  -> 帰宅後または常駐runnerが処理
  -> AIが成果物を作る
  -> 人間がHTMLやプレビューを見てGOを出す
```

html-shareをそのまま導入する必要はなく、必要なのは「依頼の入口」「成果物の確認」「GOを実行キューへ渡す経路」を一つにつなぐことだと整理された。スマホ閲覧だけなら、既存のVercel公開やClaude Artifactでも一部を満たせる。

### 2. 既存のmtg-pipelineを疑う

naruから「mtg-pipelineは実際にはワークしていない。今動いているのは問い合わせ自動対応だけ」と指摘が入った。

Claude側の調査で確認されたこと。

- mtg-pipeline workerの最終出力は2026-07-03で、その後は約45日動いていなかった
- 毎セッションのhealth-checkは停止を警告していたが、再稼働にはつながっていなかった
- 仕様やサブエージェントによる検証は充実していたが、「naruが起動し続けるか」は検証されていなかった
- 「7日連続実行で完了」というルールは、naruの実際の働き方に価値を追加していなかった

ここから、Loop Engineeringの評価軸を「仕様が正しいか」だけでなく、「naruの注意力を動力源にせず、実際の仕事の流れに組み込まれて回るか」に置き直した。

## セッションで得られた設計原則

### ループの動力源に人間の注意力を使わない

成立する起動方式は、次のどちらかに寄せる。

1. `launchd` / cron / GASなどで無人実行する
2. セッション開始、MTG終了、Gmail確認など、naruが必ず現れる業務イベントに処理を寄生させる

人間が対話セッションを開いたまま `/loop 10m` を維持する方式は、naruの仕事と席を取り合うため、Walkersの働き方には合わないと判断された。

### AIの自動化範囲と人間の承認範囲を分ける

自動化するのは、議事録の構造化、NAの抽出・登録、メールの下書きまで。メール送信や既存NAの取消など、外部への確定的な影響を持つ操作はnaruの判断に残す。

headless実行のallowlistにもとづき、`send_gmail_message` は無人実行の許可対象外にする設計が採用された。したがって「下書きを作る」と「送信する」を分離できる。

### ループは常駐することより、仕事のイベントと一致することが重要

MTGは毎分起きるイベントではなく週2〜3件程度なので、全体を常時賢く監視するより、transcript生成という確実な境界を検知する方が適している。

## 実装・検証済みの範囲

### 実装された構成

- `05_development/mtg-pipeline/watch/mtg_watch.py`
  - tl;dvを5分間隔でポーリング
  - 新規transcriptを決定論的に検知
  - LLMを待機中に呼ばず、対象MTGが見つかった時だけ `claude -p` を起動
  - 処理完了時にmacOS通知
- launchd label: `com.walkers.mtg-watch`
  - 登録・稼働開始済み
  - Macが起きている間はnaruの操作なしで監視
- 過去の50会議はseed済み
  - 過去分を掘り返さず、以後の会議を対象にする
- tl;dvのWAFがPython標準User-Agentを403にする問題を発見し、curl互換User-Agentで解消
- headless実行用に `credentials/tldv_header.txt` を作成
  - Git管理外であることを確認済み

### E2Eで確認できたこと

対象: 2026-08-17の「営業ステータス確認MTG」

- transcript: 89分、704発言
- watcher -> `claude -p` -> worker の全チェーンを実行
- 所要時間: 14分
- 議事録を生成: `03_projects/_internal/minutes/2026-08-17_営業ステータス確認MTG/minutes.md`
- 新規NA 8件、既存NA更新6件をパイプラインDBスプシに登録
- 社内MTGのため、お礼メールを作成しない判定を確認
- 処理完了のmacOS通知を確認

このテストで検証されたのは、少なくとも社内MTGの後処理が一周すること。商談後のお礼メール下書きまで含む外部商談ケースは、次の実データ検証として扱う。

## 未完了・要確認

### セッション直後から残っているもの

- 方針変更に伴い、既存NA 2件が取消推奨になった。AIは取消を実行せず、naruがスプシ上で判断する
- 中間JSON 3件が `output/mtg_6a82_*.json` に残っている。議事録に内容が保存済みだが、削除は保留
- 「次の商談で、transcript生成から15〜30分程度でお礼メール下書きまで届く」ことは期待値であり、まだ商談の実弾E2E結果ではない
- 7日連続運用とhealth-checkは、必須の完了条件から外された。ただし、運用を継続的に評価する別の観測方法は未定義

### Walkers全体のLoop Engineeringで未実装のもの

- スマホ・Windowsから依頼を入れる共通インボックス
- mission-controlの成果物プレビューからGOを出し、実行キューへ入れる経路
- `ai_edit_queue` とMac側 `claude -p` workerの一般化
- Windows常駐runnerとMac側処理の役割分担
- GitHub / Google Sheets / ローカル成果物の状態をどう正本化するか
- 失敗時の再実行、重複防止、期限切れ、ロック、監査ログ
- ループの健全性を、health-check以外の実際に役立つ形で観測する方法

## ChatGPTと再開するための壁打ち論点

次の問いを、現在のAI Workflow OSの設計に接続して検討する。

1. MTG後処理ループを、Walkersの汎用Loop Engineの最小実装とみなしてよいか。それとも単機能の自動化として切り離すべきか
2. ループの最小状態モデルは何か。少なくとも `detected -> queued -> running -> produced -> awaiting_approval -> approved/rejected -> archived` が必要か
3. transcriptの再取得やworkerの再実行が起きても、議事録・NA・下書きが重複しないためのidempotency keyは何にするか
4. 「自動化する処理」と「人間が判断する処理」の境界を案件ごとにどう宣言するか
5. Google Sheetsを承認UIとして残しつつ、GitHubを正本にする場合、MTGループの状態をどこに保存するか
6. 商談後の実E2Eを完了条件にするなら、何を測るべきか。処理時間、欠損、重複、誤判定、承認負荷、再実行時間のどれを最小セットにするか
7. `launchd`によるMTGループを、Windows/Mac間の依頼処理へ拡張する際に、どこまでを共通化し、どこからをOS固有実装にするか

## ChatGPT再開用プロンプト

以下をそのまま新しいChatGPTの壁打ち開始文として使える。

```text
WalkersでLoop Engineeringを進めています。Claude Codeの2026-08-17セッションで、まずMTG後処理の最小ループをE2E検証しました。

実装は、tl;dvのtranscript生成を5分間隔のwatcherが検知し、対象MTGについてclaude -pとworkerを起動、議事録生成・NA登録・必要ならお礼メール下書き作成を行い、最後にmacOS通知を出す構成です。launchdのcom.walkers.mtg-watchで無人稼働しています。社内MTG（89分・704発言）では、14分で完走し、議事録生成・新規NA 8件・既存NA更新6件・社内MTGなのでお礼メール対象外、まで確認しました。

一方で、これはWalkers全体のLoop Engineではありません。スマホ/Windowsから依頼を入れる共通インボックス、mission-controlからのGO、ai_edit_queue、Windows runnerとの役割分担、状態管理・再実行・重複防止・監査ログは未設計です。過去にはmtg-pipelineが約45日動いておらず、7日連続実行ルールやhealth-check警告も起動継続には効かなかったため、仕組みが存在することと実際に仕事に組み込まれて回ることを分けて考えたいです。

この前提を疑いながら、次を壁打ちしてください。

1. このMTG後処理を汎用Loop Engineの最小実装とみなすべきか
2. 最小状態モデル、idempotency key、再実行/失敗処理、人間承認境界
3. GitHubを正本、Google Sheetsをクライアント/承認ビューにする場合の状態保存先
4. 次に実装すべき1本のループと、その実データでの完了条件
5. naruの注意力を動力源にせず、実際の仕事イベントに寄り添う運用設計
```

## 次の実装判断

まず商談後の実E2Eを1件通し、下書き作成・人間承認・送信後の状態記録までを確認する。その後、同じ状態モデルと再実行規則を使える処理だけを「汎用Loop Engine」に昇格させる。最初から案件横断の自動化基盤へ広げない。
