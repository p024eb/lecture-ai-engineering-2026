# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
- 1つの質問での LLM 呼び出し回数（`requests`）：
- 追加したツールと、docstring に書いた説明：

## 宿題

- 取り組んだレベル：基礎
- やったこと：演習で見た app/agent.pyのINSTRUCTIONS や、app/tools.pyのツールのdocstring を書き換えます。同じ質問を--trace 付きで実行し、ツールの呼ばれ方や回答がどう変わるかを比べて記録する。

元の形：
  INSTRUCTIONS = """\
  あなたはAIエンジニアリング講座（AIE2026）の学習支援エージェントです。
  - 用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。
  - 回の内容を聞かれたら get_lecture_summary を使う。
  - 確認問題を求められたら make_quiz を使う。
  - 用語集に見つからないことは、推測で断定せず「用語集にはない」と伝える。
  - 回答の最後に、関係する回（第N回）を示す。
  - 宿題の答えそのものは書かず、考え方のヒントで導く。
  """
  def make_quiz(lecture: int) -> str:
      """指定した回の用語から 1 つを選び、確認問題と答えを返す。
  
      「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。
      lecture には回数（例: 2）を入れる。対象回に用語がない場合は、分かりやすく返す。
      """
問いと答え：
   uv run python -m app.agent --trace "第2回の内容から確認問 題を一問出して"
[model] gemini-3.5-flash-lite
それでは、第2回の内容から確認問題を1問出しますね！

**【確認問題】**
「LoRA」とはどのような手法でしょうか？その特徴を簡単に説明してください。

---
（答えを確認したい場合は、お気軽にお声がけください！）

第2回

--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「LoRA」の意味は？", "answer": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラメータが少なく、…
[usage] RunUsage(cost=Decimal('0.0004820'), details={'text_prompt_tokens': 990}, input_text_tokens=990, input_tokens=990, output_tokens=74, requests=2, tool_calls=1)

Instructionsの変更：
  INSTRUCTIONS = """\
  あなたはAIエンジニアリング講座（AIE2026）の学習支援エージェントです。
  - 用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。
  - 回の内容を聞かれたら get_lecture_summary を使う。
  - 確認問題を求められたら make_quiz を使う。(第2回の内容から確認問題を作る際はmake_quizを使わない。)
  - 用語集に見つからないことは、推測で断定せず「用語集にはない」と伝える。
  - 回答の最後に、関係する回（第N回）を示す。
  - 宿題の答えそのものは書かず、考え方のヒントで導く。
  """
問いと答え：
uv run python -m app.agent --trace "第2回の内容から確認問 題を一問出して"
[model] gemini-3.5-flash-lite
第2回の内容からの確認問題ですね。

**【第2回 確認問題】**
大規模言語モデル（LLM）のプロンプトエンジニアリングにおいて、モデルに具体的な事例をいくつか提示して出力の精度やフォーマットを制御する手法を何と呼ぶでしょうか？

（ヒント：ゼロから教えるのではなく、いくつかの「例（例示）」を見せて学習させる手法です。）

---
第2回

--- trace ---
[usage] RunUsage(cost=Decimal('0.0003647'), details={'text_prompt_tokens': 449}, input_text_tokens=449, input_tokens=449, output_tokens=92, requests=1)

toolsのdocstringの変更：
def make_quiz(lecture: int) -> str:
    """指定した回の用語をまとめて選び、確認問題と答えを返す。

    「第3回の確認問題を出して」のように、復習用の問題を求められたときに使う。
    lecture には回数（例: 2）を入れる。対象回に用語がない場合は、分かりやすく返す。
    """
 uv run python -m app.agent --trace "第2回の内容から確認問 題を一問出して"
[model] gemini-3.5-flash-lite
それでは、第2回の内容から確認問題を出題します！
問いと答え：
**【確認問題】**
「ランク（LoRAの $r$）」の意味は何でしょうか？

（考えてみてくださいね。準備ができたら答え合わせをしましょう！）

第2回

--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「ランク（LoRAのr）」の意味は？", "answer": "LoRAで使う低ランク行列の大きさを決めるハイパーパラメータ。大きいほど表現力が上がるが、学習するパラメータとメモリも増える。"}…
[usage] RunUsage(cost=Decimal('0.0004550'), details={'text_prompt_tokens': 950}, input_text_tokens=950, input_tokens=950, output_tokens=68, requests=2, tool_calls=1)

- 結果：instuctionでツールを呼ばないようにするとツールが呼ばれなくなった。toolsのdocstringで「第3回の確認問題を出して」のように、例示の回数を変えても元々と同じようにツールが呼ばれた。
- 気づき・疑問：instructionはモデルによって遵守されている。例示を変えてもモデルは惑わされない
