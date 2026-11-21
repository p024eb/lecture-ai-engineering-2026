# 第5回 解答

## 演習

- 回答モデル（実行時の`[model]`）： gemini-3.5-flash-lite
- 比較した質問：LoRAとQLoRAの違いは？
- 対象資料（全文投入と他の方式で範囲が違う点も記録）：data/rag_data/index.sqlite3

### 各方式の結果

| 方式 | 回答の要点・正しさ | 引用ページと根拠 | 時間（秒） | 入力／出力トークン数 | モデル応答回数 | 生成API推定費用（USD） |
|---|---|---|---|---|---|---|
| 全文投入 |要点◯・正しさ◯ | 第2回・PDF p.51, p.52|   3.58|117,025/232 | 1| $0.035688 USD|
| ベクトル検索 | 要点◯・正しさ◯ | 第2回・PDF p.51, p.52|  3.12|2,993/219 | 2| $0.001445 USD|
| Tree |要点▲・正しさ◯ |第2回・PDF p.52 | 13.37| 9,134/392 | 5| $0.003720 USD|
| Graph | 要点◯・正しさ◯ | 第2回・PDF p.51, p.52|  3.85| 5,739/296 | 2| $0.002462 USD|

<!-- 未実行・算定不可の場合は、その旨を書いてください。質問を追加した場合は、質問ごとに表を分けてください。 -->

- 検索だけを実行して見つかったページと、その本文で確認できたこと：
============================================================
検索結果 1
============================================================
講義: 基盤モデルの仕組み
章: 選んだモデルを、いつ・どう直すか
タイトル: LoRA と QLoRA ：大きなモデルも GPU 1 台で調整できる
ページ: 52

4/4 　選んだモデルを、いつ・どう直すか
LoRA と QLoRA ：大きなモデルも GPU 1 台で調整できる
元の重みは固定し、小さな「差分」だけを学習する（ LoRA ）。さらに元の重みを 4bit に圧縮する（ QLoRA ）
出典： E. J. Hu et al. 「 LoRA 」（ ICLR 2022 ）／ T. Dettmers et al. 「 QLoRA 」（ NeurIPS 2023 ）。演習では transformers ・ peft ・ bitsandbytes ・ TRL を使用
LoRA （ Hu et al. Fig.1 ）： A と B だけを学習する
青：元の重み W
固定して学習しない。 QLoRA では
4bit （数値を粗く保存）にして
約 1/4 。 7B なら約 4GB
橙：小さな行列 A ・ B
これだけを学習。細長い 2 つの
行列の積で、変化分を表す（低ランク）
イメージ
本体はそのままに、薄い「付箋」を
書き足す。学習するのは全体の
1% 未満にもできる
650 億
パラメータのモデルを
48GB GPU 1 台
で微調整（ QLoRA 論文）
• Colab の GPU （例： 16GB ）でも、
数十億パラメータ級なら調整できる。今日の
演習はこれ
• データの準備・評価・運用の手間は別にかかる
© Matsuo-Iwasawa Lab, The University of Tokyo 52

【図表・画像内の補足文字起こし】
LoRA図：Pretrained Weights W ∈ R^(d×d)、x、h、A ~ N(0, σ²)、B = 0、d、r。
- `--trace`で見えたツール呼び出し（ツール名・引数・回数、Treeでたどった目次など）：[tool call]   browse_tree({})
[目次を開く] AIエンジニアリング実践講座2026 資料
  直下の候補（ページは先頭文のみ）:
    - lec01-main · AIエンジニアリング概論とAIコーディング
    - lec02-main · 基盤モデルの仕組み
    - lec03-main · AIエージェント
[tool call]   browse_tree({'node_id': 'lec02-main'})
[目次を開く] AIエンジニアリング実践講座2026 資料 → 基盤モデルの仕組み
  直下の候補（ページは先頭文のみ）:
    - lec02-main-p001 · PDF p.1 · 基盤モデルの仕組み
      AI エンジニアリング講座 第 2 回
    - lec02-main-p002 · PDF p.2 · 今日は「 [3] モデル学習」の中身、基盤モデルそのものを扱う
      前回の振り返り
    - lec02-main-p003 · PDF p.3 · チャットボットを作ると、すぐにこんな「厄介さ」に出会う
      今日の問い
    - lec02-main-p004 · PDF p.4 · 今日の 4 つの問い（各 15 分）
      今日の構成
    - lec02-main-p005 · PDF p.5 · 今日扱うのは「モデル」そのもの
      今日の構成
    - lec02-main-p006 · PDF p.6 · 今日の用語：先に 6 つだけ押さえる
      今日の構成
    - lec02-main-s007 · LLM は、確率で次の言葉を選ぶ
    - lec02-main-s019 · 渡す・選ぶ／直す／考えさせる
    - lec02-main-s030 · 基盤モデルの地図と選び方
    - lec02-main-s045 · 選んだモデルを、いつ・どう直すか
    - lec02-main-s063 · 補足：最新動向と詳しい説明
[tool call]   browse_tree({'node_id': 'lec02-main-s045'})
[目次を開く] AIエンジニアリング実践講座2026 資料 → 基盤モデルの仕組み → 選んだモデルを、いつ・どう直すか
  直下の候補（ページは先頭文のみ）:
    - lec02-main-p045 · PDF p.45 · 選んだモデルを、いつ・どう直すか
      第 2 回　 4/4 （約 15 分）
    - lec02-main-p046 · PDF p.46 · 小規模な FT は振る舞いに強く、知識の追加は苦手
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p047 · PDF p.47 · 根拠と諸説：知識の追加は RAG が効きやすい
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p048 · PDF p.48 · 継続事前学習（ CPT ）：知識・言語を足す追加学習
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p049 · PDF p.49 · FT は「コストを下げる」ためにも使う
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p050 · PDF p.50 · 判断の順番：測る → 分ける → プロンプト → Fine-Tuning
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p051 · PDF p.51 · PEFT ：モデル全体ではなく、一部だけを学習する
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p052 · PDF p.52 · LoRA と QLoRA ：大きなモデルも GPU 1 台で調整できる
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p053 · PDF p.53 · チャットボットの場合：何を FT し、何を FT しないか
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p054 · PDF p.54 · 手本データの作り方：質問と、理想の回答の組を用意する
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p055 · PDF p.55 · FT の落とし穴：直したところ以外が、壊れていないか
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p056 · PDF p.56 · クイズ 4 ： Fine-Tuning が一番向いているのは？
      クイズ 4/4
    - lec02-main-p057 · PDF p.57 · クイズ 4 ： Fine-Tuning が一番向いているのは？（答え）
      クイズ 4/4
    - lec02-main-p058 · PDF p.58 · Fine-Tuning する前のチェックリスト
      4/4 　選んだモデルを、いつ・どう直すか
    - lec02-main-p059 · PDF p.59 · 第 2 回のまとめ： 4 つの芯と、明日やること
      まとめ
    - lec02-main-p060 · PDF p.60 · 今日の演習： QLoRA で出力形式を矯正し、効果を測る
      演習
    - lec02-main-p061 · PDF p.61 · 演習の進め方と提出物（仮）
      演習
    - lec02-main-p062 · PDF p.62 · 次回： AI エージェント。今日の性質が設計の前提になる
      次回予告
[tool call]   browse_tree({'node_id': 'lec02-main-p052'})
[目次を開く] AIエンジニアリング実践講座2026 資料 → 基盤モデルの仕組み → 選んだモデルを、いつ・どう直すか → LoRA と QLoRA ：大きなモデルも GPU 1 台で調整できる
    （末端ページ・本文717文字を取得）
- 取得した情報が足りなかった例、取得できたのに回答へ使えなかった例：特になし。
- 今回の用途に採用したい方式と、その理由・比較条件の限界、気づき・疑問：：金額、回答速度、回答の正しさ・要点的にベクトル検索で良い。ベクトル検索だったら、特定の語が入っていたら意味が逆でも（例えば質問がLoRAの欠点は？だったらLoRAの長所のページも）ヒットしてしまう可能性があるかもしれない。そのその検討はしていない。


## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎 
- やったこと（変更したファイル・設定・処理、または設計した取得方法）：app/config.pyのRAG_TOP_K=5を1に変更した。
- 比較した質問と、固定した条件（モデル・対象資料など）：gemini-3.5-flash-lite、対象資料はdata/rag_data/index.sqlite3

- 結果（変更前後の回答・根拠、`--trace`の出力、時間・トークン数・生成API推定費用など）：
(変更前) @p024eb ➜ /workspaces/lecture-ai-engineering-2026 (lec05) $ uv run python -m app.agent --trace "ハーネスとIn context learningの違いは？"
[model] gemini-3.5-flash-lite
講義資料において、**ハーネス（Harness）**と**インコンテキスト学習（In-context learning）**はそれぞれ異なる概念・仕組みとして説明されています。

それぞれの役割や定義の違いについて、資料の記載を基に整理します。

---

### 1. ハーネス（Harness）とは
* **定義・役割**：
  AIエージェントにおいて、LLMモデルと組み合わせて動作するシステム全般を指します（「エージェント ＝ モデル ＋ ハーネス」）第3回・PDF p.14。
  具体的には、以下のような役割を持つプログラムや仕組みをまとめてハーネスと呼びます。
  * コンテキストの組み立て（履歴・メモリ・ツール一覧などの管理）第3回・PDF p.14
  * ツールの実行（プログラムを実行し、その結果をコンテキストに戻す）第3回・PDF p.14
  * 記録の保持・メモリへの要点書き出し第3回・PDF p.14
  * 権限の確認や安全性の担保（危ない操作の承認、認証情報の隔離など）第3回・PDF p.14, 68

---

### 2. インコンテキスト学習（In-context learning）について
* 第1〜3回の講義資料の本文中において、専門用語としての「In-context learning（インコンテキスト学習）」という言葉そのものは定義・解説されていません。
* ただし、モデルに対する入力（コンテキスト）の仕組みに関連して、以下のような解説があります。
  * チャットでは、これまでのやり取りの全体（コンテキスト）を毎回すべて送り直していること第3回・PDF p.8
  * コンテキストには、会話だけでなくシステムプロンプトやツールの説明なども含まれること第3回・PDF p.9
  * モデルの一度に読める長さが伸びても、長い文脈の中央付近の情報は見落とされやすい特性（Lost in the Middle）があること第2回・PDF p.15

---

### 違いのまとめ
* **ハーネス**は、LLMの周囲で「コンテキストを組み立てる」「外部ツールを実行する」「メモリや安全性を管理する」といった**システム・プログラムの枠組み全体**を指します（第3回・PDF p.14）。
* 一方、LLMへの入力や文脈の扱いに着目する場合、モデルがプロンプトやコンテキスト（入力データ）を受け取って処理する仕組みそのものが議論されます。ハーネスは、そのコンテキストを動的に組み立てたり管理したりする外側のシステムとして機能します（第3回・PDF p.14）。

--- trace ---
[tool call]   search_materials({'query': 'ハーネス'})
[tool call]   search_materials({'query': 'In context learning'})
[tool return] search_materials -> {"results": [{"page_id": "lec03-main-p014", "document_id": "lec03-main", "lecture": 3, "document_title": "AIエージェント", "se…
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p024", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[tool call]   search_materials({'query': 'In-context learning'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p024", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[tool call]   search_materials({'query': 'インコンテキスト'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p015", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[time] 回答完了まで 7.93 秒（検索・埋め込みを含む）
[usage] 入力 19,317 tokens / 出力 650 tokens / モデル応答 4 回
[cost] 生成APIの推定コスト: $0.004682 USD（埋め込み・索引構築分は含まない）

(変更後) @p024eb ➜ /workspaces/lecture-ai-engineering-2026 (lec05) $ uv run python -m app.agent --trace "ハーネスとIn context learningの違いは？"
[model] gemini-3.5-flash-lite
講義資料において、「ハーネス」は次のように説明されています。

* **ハーネス**：
  エージェント＝モデル＋ハーネスという構成において、コンテキストを組み立てたり（履歴・メモリ・ツール一覧）、ツールを実行して結果をコンテキストに戻したり、記録を残して要点をメモリに書き出したり、危ない操作の承認のために権限を確かめてツールを止めたりするシステムのことです（第3回・PDF p.14）。

一方、ご質問の「In-context learning」については、提供された第1〜3回の講義資料内には該当する用語や定義の記載を確認できませんでした。そのため、資料に記載されている範囲でお答えできるのはハーネスの役割・機能までとなります。

--- trace ---
[tool call]   search_materials({'query': 'ハーネス'})
[tool return] search_materials -> {"results": [{"page_id": "lec03-main-p014", "document_id": "lec03-main", "lecture": 3, "document_title": "AIエージェント", "se…
[tool call]   search_materials({'query': 'In context learning'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p024", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[tool call]   search_materials({'query': 'In-context learning'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p024", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[tool call]   search_materials({'query': 'コンテキスト'})
[tool return] search_materials -> {"results": [{"page_id": "lec03-main-p013", "document_id": "lec03-main", "lecture": 3, "document_title": "AIエージェント", "se…
[tool call]   search_materials({'query': 'ICL'})
[tool return] search_materials -> {"results": [{"page_id": "lec03-main-p024", "document_id": "lec03-main", "lecture": 3, "document_title": "AIエージェント", "se…
[time] 回答完了まで 8.87 秒（検索・埋め込みを含む）
[usage] 入力 7,940 tokens / 出力 244 tokens / モデル応答 6 回
[cost] 生成APIの推定コスト: $0.002992 USD（埋め込み・索引構築分は含まない）
- 改善した例と、残る失敗・制約：In-context learningという単語自体は検索対象資料内にないため、RAGを検索範囲をtop5から1にすると、検索回数が増えるとともに、該当する語がなかったために語に対する説明を生成できなかった。
- 気づき・疑問：変更前は該当する語がなかったとしても関連する上位5位までの検索結果が返されたため、語そのものの説明はなかったが語に関連する情報を得られた。しかし、AIの回答の読みやすさとしては余計な情報が含まれるため、ノイズとなる場合がありそうだと感じた。
