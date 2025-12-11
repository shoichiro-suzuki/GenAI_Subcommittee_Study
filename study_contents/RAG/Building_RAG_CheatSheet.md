# A Cheat Sheet and Some Recipes For Building Advanced RAG

![チートシート](https://cdn.sanity.io/images/7m9jw85w/production/6cfb645c4e5dfea8f3e5a590c3d2bc1cbfcfead3-5818x7805.png?w=5818)

---
## 1. Motivation（なぜRAGが必要？）

左上の「Motivation」では、**LLMだけでQAをするとダメな理由**が整理されています。

* LLMは **幻覚（hallucination）** を起こす
* 必要な情報が、LLMの学習コーパスの外にあることが多い
* 学習後に発生した **最新情報にアクセスできない**

そこで「事前学習済みLLMだけで答えさせるのではなく、外部知識を検索してから使おう」という発想がRAGです。

---

## 2. Basic RAG の構造

中央上の「Basic RAG」ブロックが、一番素朴なRAGの流れです。

1. ユーザの質問（Question）
2. **Retriever** が質問をもとに外部知識ベースから
   「Top-k類似ドキュメント」を検索する
3. 質問 + 取得したドキュメント群 を **Generator（LLM）** に渡す
4. LLMが最終的な Answer を生成

その下には、外部ナレッジを作る4ステップも載っています。

* Extract / Parse（データ取り込み・解析）
* Chunk（分割）
* Embeddings（ベクトル化）
* Index（インデックス構築）

右側の「Relevant Abstractions」は、LlamaIndexのクラス名（VectorStoreIndex, Document, Node, BaseRetriever など）が例として書かれていて、「実装上はこういうコンポーネントで分かれてますよ」ということを示しています。

### Basic RAGで満たすべき2つの大前提

黄色い付箋に書かれているのは、RAG成功のための**2つのトップレベル要件**です。

1. **Retrieval must be able to find the most relevant documents.**
   → 検索結果がズレていたら、その後どれだけ頑張ってもムダ。
2. **Generation must be able to make good use of the retrieved documents.**
   → 文書をちゃんと読んで、根拠に基づいて答えを組み立てられること。

RAGの議論は、究極的にはいつもこの2つに還元できます。

---

## 3. Key Abilities & Quality Scores

右上のオレンジの欄は、「良いRAGシステムに必要な能力」と「評価指標」です。

### Key Abilities（能力）

* **Noise Robustness**
  リトリーブされたコンテキストの中に、ノイズや関係ない情報が混ざっていても壊れない力
* **Negative Rejection**
  知識が足りないときに、変に作り話をせず「わからない」と言える力
* **Information Integration**
  複数のドキュメントから情報を統合して、複雑な質問に答える力
* **Counterfactual Robustness**
  間違った情報（誤情報）がコンテキストに入っていても、それを見抜いたり扱える力

### Quality Scores（評価指標）

* **Context Relevance**
  取ってきた文書は、質問に本当に関係しているか？
* **Answer Relevance**
  生成された答えは、質問にちゃんと答えているか？
* **Faithfulness**
  答えは、コンテキストに忠実か？（幻覚していないか？）

ここは「どう設計するか」だけでなく、「どう評価するか」を示していて、実運用ではかなり重要な視点です。

---

## 4. Advanced RAG（高度なRAGテクニック）

下半分は **Advanced RAG** で、上の2つの要件をどう強化するかが
「検索側」「生成側」「両方まとめて」の3ブロックに分かれています。

### 4-1. 検索（Retrieval）を強化する手法

左下大きな枠の中：

* **Chunk-size optimization**
  チャンクが大きすぎても小さすぎても精度が落ちるので、
  いいサイズに調整する（NodeParser, ParamTuner）。
* **Sliding window chunking**
  少しずつ重なりを持たせて分割し、長文の「切れ目問題」を減らす。
* **Structured knowledge**
  文書を構造化（グラフ・ツリーなど）して、再帰的な検索やルーティングを可能にする。
* **Metadata attachments**
  メタデータ（キーワード・タグ・属性）を付けて、フィルタ検索や絞り込みをやりやすくする。
* **Knowledge graphs**
  エンティティと関係をグラフで表現し、推論しやすい形でナレッジを保存する。
* **Mixed retrieval**
  キーワード検索とベクトル検索を組み合わせて、質問に応じた柔軟な検索を行う。
* **Embedding fine-tuning**
  域固有コーパスで埋め込みモデルを微調整し、類似度の精度を上げる。
* **Question–Embedding Transformation**
  HyDEなどの手法で、質問側の埋め込みを「文書側に近づくよう」変換して、ヒット率を改善する。

### 4-2. 生成（Generation）を強化する手法

左下の別枠：

* **Information Compression**
  要約・圧縮して、ノイズ削減やコンテキスト長の制約を緩和（LongLLMLingua 系など）。
* **Generator Fine-Tuning**
  LLM自体をRAGタスク向けに微調整して、コンテキストの使い方をうまくする（Self-RAG など）。
* **Result Re-Rank**
  複数の候補回答を作ってリランキングし、「真ん中に大事な情報が埋もれる」問題を軽減。
* **Adapter Methods**
  アダプタレイヤーなどを使い、外部から生成結果を修正するポストプロセッサをかませる。

### 4-3. 検索と生成をまとめて良くする手法

右下の枠：

* **Monolithic Fine-Tuning**
  Retriever と Generator をまとめて end-to-end で微調整（RA-DIT など）。
* **Retrieval Foundational Models**
  最初から「検索前提のアーキテクチャ」でLLMを事前学習（RETRO など）。
* **Generator-Enhanced Retrieval**
  LLM自身に「質問の言い換え・分解・ステップバック思考」をさせて、検索を賢くする（FLAREなど）。
* **Iterative Retrieval-Generation**
  生成と検索を何度もループさせて、推論を深めるエージェント的なRAG（RetryQueryEngine など）。

---

## 5. このチートシートの読み方・活かし方

ざっくり言うと、この図はこういうメッセージです：

* RAGは「ベクトルDBをつないだら終わり」ではなく、
  **検索の質** と **生成の使い方** を別々に、かつ一緒にも改善していく必要がある。
* 問題が起きたときに
  「これは Retrieval 側の問題か？ Generation 側か？ それとも両方か？」
  を切り分けるためのマップになっている。
* LlamaIndexを使う場合、各ブロックの下に書かれているクラス名を
  そのまま実装の出発点として使える。

社内ナレッジ検索や手順書検索のRAGを作るなら、
まずは Basic RAG で動かしてみて、
「再現性の低さ＝どの能力・どのスコアがボトルネックか」を見極め、
この図の中から手を打つ場所を選ぶ、という使い方がしやすいはずです。

---

[原文：A Cheat Sheet and Some Recipes For Building Advanced RAG](https://www.llamaindex.ai/blog/a-cheat-sheet-and-some-recipes-for-building-advanced-rag-803a9d94c41b)

---
