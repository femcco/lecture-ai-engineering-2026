# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
- 1つの質問での LLM 呼び出し回数（`requests`）：
- 追加したツールと、docstring に書いた説明：

## 宿題

- 取り組んだレベル：基礎 / 実装 / 発展
  基礎

- やったこと：
app/agent.pyのINSTRUCTIONSを変更した。
具体的には、
- 擁護や講座に固有のことを考える前に、必ず search_glossary で用語集を確認する。
という記述を外して、元の設定での出力と、外した後での出力を比較した。

- 結果：
（元の設定）
LoRAとQLoRAの違いについて、講座の用語集を基に解説します。

* **LoRA（Low-Rank Adaptation）**
  事前学習済みの重みを固定したまま、重みの更新分を2つの小さな低ランク行列の積で近似し、その部分だけを学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習するパラメータ数が少なく、小さなファイルサイズでアダプタを保存できるのが特徴です。

* **QLoRA（Quantized LoRA）**
  LoRAの仕組みをベースにしつつ、**ベースモデルの重みを4ビットなどの低ビットに量子化**して読み込むことで、GPUのメモリを大幅に節約できるようにした手法です。これにより、より大きなモデルを限られたGPUメモリ環境でも扱えるようになります。

簡単にまとめると、LoRAは「低ランク行列を用いて効率的に学習する手法」であり、QLoRAは「ベースモデルを量子化してさらにメモリ使用量を削りつつ、LoRAで学習する手法」です。

関連する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0010288'), details={'text_prompt_tokens': 1196}, input_text_tokens=1196, input_tokens=1196, output_tokens=268, requests=2, tool_calls=2)

（外した後）
LoRAとQLoRAの違いは、ベースモデルの重みをそのまま使うか、量子化してメモリを節約するかにあります。

* **LoRA**: 事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法です。学習するパラメータが少なく、アダプタと呼ばれる小さなファイルで保存できます。
* **QLoRA**: ベースモデルの重みを4ビットなどに量子化（Quantization）してメモリを大きく節約したうえで、LoRAによる学習を行う手法です。これにより、より大きなモデルを小さなGPU環境でも扱えるようになります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008394'), details={'text_prompt_tokens': 1198}, input_text_tokens=1198, input_tokens=1198, output_tokens=192, requests=2, tool_calls=2)

- 気づき・疑問：
「必ず search_glossary で用語集を確認する。」というINSTRACUTIONSを外しても、toolとして、search_glossaryを認識しているので、きちんとLoRAおよびQLoRAをsearch_glossaryで確認して、正確な出力ができることがわかった。
