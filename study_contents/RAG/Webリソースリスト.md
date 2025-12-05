## 1. データベース作成フェーズ

### 1.1 ドキュメント取り込み戦略

#### 電子化する情報の範囲：マルチモーダル対応

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| An Easy Introduction to Multimodal RAG | https://developer.nvidia.com/blog/an-easy-introduction-to-multimodal-retrieval-augmented-generation/ | 英語 | NVIDIA公式。CLIP活用、画像キャプショニング、複数ストア+リランカーの3アプローチ |
| マルチモーダルRAGを作ってみた | https://dev.classmethod.jp/articles/multimodal-rag-chatbot/ | 日本語 | Amazon Titan、Azure AI Vision、Vertex AIのマルチモーダル埋め込みAPI比較 |
| Multi-Vector Retriever for RAG on tables, text, and images | https://blog.langchain.com/semi-structured-multi-modal-rag/ | 英語 | テーブル・テキスト・画像を含む半構造化データのRAG構築手法 |

#### メタデータ付与：意図と効果

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAGでの回答精度向上のためのテクニック集 | https://zenn.dev/knowledgesense/articles/cec1cd43244524 | 日本語 | メタデータ付加によるフィルタリング、Self-queryingの実践テクニック |
| RAGの性能を改善するための8つの戦略 | https://fintan.jp/page/10301/ | 日本語 | メタデータフィルタリング（日付・位置情報等）、PDFTriageによる文書構造活用 |

#### 文章の正規化

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| The Role of Preprocessing in RAG | https://www.deepset.ai/blog/preprocessing-rag | 英語 | テキスト正規化（空白除去、ヘッダー/フッター処理）の体系的解説 |
| 日本語テキストの前処理 | https://tuttieee.hatenablog.com/entry/ja-nlp-preprocess | 日本語 | neologdnによる表記ゆれ吸収、半角/全角統一など実践的手法 |
| RAG検索は前処理×チャンクで決まる | https://www.openbridge.jp/column/rag-preprocess-chunking | 日本語 | 5ステップの前処理フローとメタデータスキーマ設計 |

#### 用語の統一：ベストプラクティス

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAGに適したデータをつくる前処理技術とは？ | https://note.com/ilujapan/n/na2d767631153 | 日本語 | シノニムマップ自動生成、表記ゆれ・言い換え表現の吸収 |
| Taming the Chaos: Data Normalization for RAG | https://medium.com/@anuva_74249/taming-the-chaos-datanormalization-for-llm-applications-ba8575c59725 | 英語 | 異なるフォーマット（HTML、PDF等）のインテリジェント正規化 |

#### 【調査結果】その他のドキュメント取り込み戦略

**OCR・レイアウト解析**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAG with Azure Document Intelligence | https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/concept/retrieval-augmented-generation | 英語 | **Microsoft公式**。Layout modelによる文書構造解析、309言語対応OCR |
| Transform RAG with Azure AI Document Intelligence | https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/elevating-rag-and-search-the-synergy-of-azure-ai-document/ba-p/4006348 | 英語 | 段落・表・見出し・数式抽出、Markdown出力 |
| Azure AI Searchの「マルチモーダルRAG」を試してみた | https://www.qes.co.jp/media/azure/aisearch/a562 | 日本語 | 画像言語化、Document Intelligence Layout Skillの実践 |

**Document Intelligence・構造解析**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Prep your Data for RAG with Azure AI Search | https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/prep-your-data-for-rag-with-azure-ai-search-content-layout-markdown-parsing--imp/4303538 | 英語 | Document Layout Skillによる構造認識チャンキング |
| VISRAG: マルチモーダル文書における視覚ベースのRAG | https://note.com/shimmyo_lab/n/n84f93b9e5ae4 | 日本語 | 最新研究VisRAG。ページ画像を直接扱うVision-Language Model活用 |

---

### 1.2 エンベディングモデル選定戦略

#### モデル選定ガイド

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Choosing an Embedding Model | https://www.pinecone.io/learn/series/rag/embedding-models-rundown/ | 英語 | **Pinecone公式**。Ada 002、Cohere、E5の比較、MTEBリーダーボード活用法 |
| Step-by-Step Guide to Choosing the Best Embedding Model | https://weaviate.io/blog/how-to-choose-an-embedding-model | 英語 | **Weaviate公式**。タスク別選定基準、次元数・トークン数のトレードオフ |
| How to Choose the Right Embedding Model for RAG | https://milvus.io/blog/how-to-choose-the-right-embedding-model-for-rag.md | 英語 | コンテキストウィンドウ、トークナイゼーション方式の影響 |

#### 日本語対応モデルの比較

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| PLaMo-Embedding-1Bの開発 | https://tech.preferred.jp/ja/blog/plamo-embedding-1b/ | 日本語 | **PFN公式**。JMTEBでtext-embedding-3-largeを上回る日本語モデル |
| RAGにおける埋め込みモデルの比較 | https://note.com/alexweberk/n/ncccfdab3f4bb | 日本語 | multilingual-e5-large、SimCSE、GLuCoSE、Ada-002の実践比較 |
| RAGの性能を向上させるEmbedding Modelの選択 | https://www.netone.co.jp/media/detail/20240823-01/ | 日本語 | Cohere、OpenAI、オープンモデルのRAG精度への影響分析 |

---

### 1.3 チャンキング戦略

#### 固定長チャンキング（文字数・トークン数ベース）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| How to recursively split text by characters | https://python.langchain.com/v0.2/docs/how_to/recursive_text_splitter/ | 英語 | **LangChain公式**。RecursiveCharacterTextSplitterの実装例 |
| Node Parser Modules | https://docs.llamaindex.ai/en/stable/module_guides/loading/node_parsers/modules/ | 英語 | **LlamaIndex公式**。SentenceSplitter、TokenTextSplitter等 |
| RAGにおけるチャンクとは？ | https://ai-market.jp/technology/rag-chunk/ | 日本語 | チャンキングの基礎から応用まで網羅的に解説 |

#### セマンティックチャンキング

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Semantic Chunker - LlamaIndex | https://developers.llamaindex.ai/python/examples/node_parsers/semantic_chunking/ | 英語 | 埋め込み類似度に基づく意味分割の実装例 |
| Semantic Chunking - VectorHub | https://superlinked.com/vectorhub/articles/semantic-chunking | 英語 | 3種類のセマンティックチャンキング手法の比較評価 |
| Improving RAG Performance: WTF is Semantic Chunking? | https://www.fuzzylabs.ai/blog-post/improving-rag-performance-semantic-chunking | 英語 | コサイン距離を用いた分割境界検出の原理 |

#### 【調査結果】その他のチャンキング戦略

**オーバーラップチャンキング**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Chunking Strategies for RAG - Weaviate | https://weaviate.io/blog/chunking-strategies-for-rag | 英語 | chunk_overlap（10-20%推奨）の設定方法 |
| Chunking Strategies for AI and RAG - DataCamp | https://www.datacamp.com/blog/chunking-strategies | 英語 | Sliding Window Chunking、20-50%オーバーラップの推奨値 |

**階層的チャンキング（Parent-Child Chunking）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| How to use the Parent Document Retriever | https://python.langchain.com/docs/how_to/parent_document_retriever/ | 英語 | **LangChain公式**。子チャンク検索→親チャンク取得の実装 |
| Advanced RAG 01: Small-to-Big Retrieval | https://medium.com/data-science/advanced-rag-01-small-to-big-retrieval-172181b396d4 | 英語 | Small-to-Big検索の詳細解説と実装例 |
| Parent-child Retrieval - Dify | https://dify.ai/blog/introducing-parent-child-retrieval-for-enhanced-knowledge | 英語 | 製品での実装例、精度向上メカニズム |

**Late Chunking（後分割手法）**— 2024年Jina AI発表の革新的手法

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Late Chunking in Long-Context Embedding Models | https://jina.ai/news/late-chunking-in-long-context-embedding-models/ | 英語 | **Jina AI公式**。文書全体を先に埋め込み、後でチャンク分割 |
| Late Chunking: Contextual Chunk Embeddings (論文) | https://arxiv.org/pdf/2409.04701 | 英語 | 従来手法との比較評価のエビデンス |
| Late Chunking for RAG: Implementation With Jina AI | https://www.datacamp.com/tutorial/late-chunking | 英語 | ステップバイステップの実装チュートリアル |

**RAPTOR（Recursive Abstractive Processing for Tree-Organized Retrieval）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAPTOR原論文 | https://arxiv.org/html/2401.18059v1 | 英語 | 再帰的クラスタリングと要約によるツリー構造構築 |
| Improving RAG with RAPTOR | https://superlinked.com/vectorhub/articles/improve-rag-with-raptor | 英語 | Tree Traversal vs Collapsed Treeの比較 |
| Mastering RAG with RAPTOR using LlamaIndex | https://www.educative.io/blog/mastering-rag-with-raptor | 英語 | LlamaIndexでの実装チュートリアル |

**Agentic Chunking（LLM構造認識チャンキング）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| The Ultimate Guide to Chunking Strategies - Databricks | https://community.databricks.com/t5/technical-blog/the-ultimate-guide-to-chunking-strategies-for-rag-applications/ba-p/113089 | 英語 | AI-driven Dynamic Chunkingを含む包括的ガイド |
| RAGシステム最適化のためのチャンキング手法比較 | https://note.com/aivalix/n/nc02bd87d2ddd | 日本語 | 4つのチャンキング手法の実装比較 |

**総合ガイド**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| 15 Chunking Techniques to Build Exceptional RAGs | https://www.analyticsvidhya.com/blog/2024/10/chunking-techniques-to-build-exceptional-rag-systems/ | 英語 | 15種類のチャンキングテクニック網羅 |
| RAG ソリューションの開発 - チャンキング フェーズ | https://learn.microsoft.com/ja-jp/azure/architecture/ai-ml/guide/rag/rag-chunking-phase | 日本語 | **Microsoft公式**。境界ベース、カスタムコード、文書分析モデル |

---

## 2. Retrievalフェーズ

### 2.1 検索戦略

#### ベクトル検索（Vector Search）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Vector Search and RAG Tutorial | https://www.freecodecamp.org/news/vector-search-and-rag-tutorial-using-llms-with-your-data/ | 英語 | 3つのプロジェクトを通じたベクトル検索からRAG実装 |
| RAG with MongoDB Atlas Vector Search | https://www.mongodb.com/docs/atlas/atlas-vector-search/rag/ | 英語 | データ取り込み、インデックス作成、検索実装までの手順 |
| RAGを支える技術 ー ベクトル検索の仕組み | https://qiita.com/yuji-arakawa/items/c60251131c00f2ac02f1 | 日本語 | 埋め込みモデルからベクトルDBまで体系的に解説 |

#### フルテキスト検索（BM25）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| BM25 relevance scoring - Azure AI Search | https://learn.microsoft.com/en-us/azure/search/index-similarity-and-scoring | 英語 | **Microsoft公式**。BM25アルゴリズム、パラメータ調整方法 |
| Understanding the BM25 full text search algorithm | https://emschwartz.me/understanding-the-bm25-full-text-search-algorithm/ | 英語 | BM25の数学的基礎と動作原理 |
| Practical BM25 - Elasticsearch | https://www.elastic.co/blog/practical-bm25-part-2-the-bm25-algorithm-and-its-variables | 英語 | **Elastic公式**。k1、bパラメータの実践的解説 |

#### セマンティック検索（Semantic Search）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Semantic search with FAISS - Hugging Face | https://huggingface.co/learn/llm-course/chapter5/6 | 英語 | sentence-transformersとFAISSの実装 |
| Building a semantic search engine in OpenSearch | https://opensearch.org/blog/semantic-search-solutions/ | 英語 | プリトレイン・カスタムモデル両方の実装 |

#### メタデータフィルター

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Enhancing RAG with Metadata: Self-Query Retrievers | https://medium.com/@lorevanoudenhove/enhancing-rag-performance-with-metadata-the-power-of-self-query-retrievers-e29d4eecdb73 | 英語 | LangChain+Pineconeでのメタデータフィルタリング実装 |

#### 【調査結果】その他の検索戦略

**ハイブリッド検索（Hybrid Search）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| A Comprehensive Hybrid Search Guide - Elastic | https://www.elastic.co/what-is/hybrid-search | 英語 | **Elastic公式**。RRFを含む包括的ガイド |
| Improving Retrieval Performance with Hybrid Search | https://towardsdatascience.com/improving-retrieval-performance-in-rag-pipelines-with-hybrid-search-c75203c2f2f5/ | 英語 | Weaviateでのalphaパラメータ調整方法 |
| Hybrid Search: Vector + Keyword Techniques | https://www.machinelearningplus.com/gen-ai/hybrid-search-vector-keyword-techniques-for-better-rag/ | 英語 | FAISS+BM25の組み合わせ実装コード付き |

**グラフベース検索（Graph RAG / Knowledge Graph）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| GraphRAG公式ドキュメント | https://microsoft.github.io/graphrag/ | 英語 | **Microsoft Research公式**。ナレッジグラフを使った検索拡張 |
| RAG Tutorial: Build a RAG System on a Knowledge Graph | https://neo4j.com/blog/developer/rag-tutorial/ | 英語 | **Neo4j公式**。GraphRAG構築のステップバイステップ |
| Knowledge Graphs for RAG | https://www.deeplearning.ai/short-courses/knowledge-graphs-rag/ | 英語 | Andrew Ng主催のショートコース |

**HyDE（Hypothetical Document Embeddings）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| HyDE原論文 | https://arxiv.org/abs/2212.10496 | 英語 | ゼロショット検索における仮説的文書生成の理論 |
| Better RAG with HyDE - Zilliz | https://zilliz.com/learn/improve-rag-and-information-retrieval-with-hyde-hypothetical-document-embeddings | 英語 | OpenAI+Milvusでの実装例 |
| 【RAG】LangChainでHyDEを試す | https://zenn.dev/khisa/articles/cc2ff969d4f2b8 | 日本語 | 青春18きっぷ情報検索での実践的検証 |

**Multi-Query Retrieval**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| LangChain / Llama-Index: RAG with Multi-Query Retrieval | https://teetracker.medium.com/langchain-llama-index-rag-with-multi-query-retrieval-4e7df1a62f83 | 英語 | マルチクエリ検索実装、SubQuestionQueryEngine |
| LangChain Multi-Query for RAG - Pinecone | https://github.com/pinecone-io/examples/blob/master/learn/generation/langchain/handbook/10-langchain-multi-query.ipynb | 英語 | **Pinecone公式**。Jupyter Notebook実装例 |

**Self-Query Retrieval**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Building an Advanced RAG System With Self-Querying | https://www.mongodb.com/developer/products/atlas/advanced-rag-self-querying-retrieval/ | 英語 | **MongoDB公式**。メタデータスキーマ設計からLangGraph実装まで |

**Contextual Retrieval（Anthropic提唱）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Introducing Contextual Retrieval | https://www.anthropic.com/news/contextual-retrieval | 英語 | **Anthropic公式**。検索エラーを最大67%削減 |
| Contextual Retrieval: A Guide With Implementation | https://www.datacamp.com/tutorial/contextual-retrieval-anthropic | 英語 | Contextual Embeddings+BM25の組み合わせ実装 |
| Contextual retrieval using Amazon Bedrock | https://aws.amazon.com/blogs/machine-learning/contextual-retrieval-in-anthropic-using-amazon-bedrock-knowledge-bases/ | 英語 | **AWS公式**。RAGASによる評価付き |

---

### 2.2 リランク戦略

#### RRF（Reciprocal Rank Fusion）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| 検索性能を改善するランク融合アルゴリズム | https://hironsan.hatenablog.com/entry/rank-fusion-algorithms | 日本語 | RRFの数式解説、ranxライブラリ実装、他アルゴリズム比較 |
| Hybrid search scoring (RRF) - Azure AI Search | https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking | 英語 | **Microsoft公式**。k=60のスコア計算式 |
| Reciprocal rank fusion - Elasticsearch | https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion | 英語 | **Elastic公式**。BM25+ELSERの組み合わせ例 |

#### セマンティックランカー（Azure AI Search）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Semantic ranking - Azure AI Search | https://learn.microsoft.com/ja-jp/azure/search/semantic-search-overview | 日本語 | **Microsoft公式**。Bing言語理解モデル活用、課金体系 |
| Configure semantic ranker | https://learn.microsoft.com/en-us/azure/search/semantic-how-to-configure | 英語 | prioritizedFieldsの定義、SDK/REST API実装 |
| Azure AI Searchのセマンティックハイブリッド検索によるRAGの性能向上 | https://techtekt.persol-career.co.jp/entry/tech/240521_01 | 日本語 | 実務適用事例、@search.rerankerScoreの解説 |

#### 【調査結果】その他のリランク手法

**Cross-Encoder（Bi-EncoderとCross-Encoderの違い）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Cross-Encoders — Sentence Transformers | https://sbert.net/examples/cross_encoder/applications/README.html | 英語 | **公式ドキュメント**。基本概念、Bi-Encoderとの使い分け |
| Rerankers and Two-Stage Retrieval - Pinecone | https://www.pinecone.io/learn/series/rag/rerankers/ | 英語 | **Pinecone公式**。Bi-Encoder（高速）vs Cross-Encoder（高精度） |
| Using Cross-Encoders as reranker - Weaviate | https://weaviate.io/blog/cross-encoders-as-reranker | 英語 | マルチステージ検索パイプライン実装、図解付き |
| Cross Encoder Reranker - LangChain | https://python.langchain.com/docs/integrations/document_transformers/cross_encoder_reranker/ | 英語 | **LangChain公式**統合方法 |

**Cohere Rerank**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Master Reranking with Cohere Models | https://docs.cohere.com/docs/reranking-with-cohere | 英語 | **Cohere公式**。Rerank 3、半構造化データ対応 |
| End-to-end RAG with Chat, Embed, and Rerank | https://docs.cohere.com/docs/rag-complete-example | 英語 | **Cohere公式**。Embed→Rerank→Chatの完全フロー |
| Improve RAG performance using Cohere Rerank - AWS | https://aws.amazon.com/blogs/machine-learning/improve-rag-performance-using-cohere-rerank/ | 英語 | **AWS公式**。SageMaker JumpStart導入 |

**BGE Reranker（BAAI）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| BAAI/bge-reranker-v2-m3 - Hugging Face | https://huggingface.co/BAAI/bge-reranker-v2-m3 | 英語 | **公式モデルカード**。多言語対応の軽量リランカー |
| FlagOpen/FlagEmbedding - GitHub | https://github.com/FlagOpen/FlagEmbedding | 英語 | **公式リポジトリ**。BGE埋め込み・リランカーシリーズ |
| BGE Reranker Tutorial | https://bge-model.com/tutorial/5_Reranking/5.2.html | 英語 | 各モデル（base/large/v2-m3）の使い分け |

**ColBERT / ColBERTv2**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| stanford-futuredata/ColBERT - GitHub | https://github.com/stanford-futuredata/ColBERT | 英語 | **公式リポジトリ**。ColBERT/ColBERTv2の実装 |
| What is ColBERT and Late Interaction - Jina AI | https://jina.ai/news/what-is-colbert-and-late-interaction-and-why-they-matter-in-search/ | 英語 | Late Interactionの概念解説、MaxSim演算子 |
| Colbert Rerank - LlamaIndex | https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/ColbertRerank/ | 英語 | **LlamaIndex公式**。ColBERTv2リランカー実装例 |

**LLM-based Reranking / Listwise Reranking**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Using LLMs as a Reranker for RAG: A Practical Guide | https://fin.ai/research/using-llms-as-a-reranker-for-rag-a-practical-guide/ | 英語 | Pointwise vs Listwise、プロンプト公開 |
| RankZephyr: Effective Listwise Reranking (論文) | https://arxiv.org/html/2312.02724v1 | 英語 | ゼロショットリランキング、RankGPT4との比較 |
| Mastering RAG: How to Select A Reranking Model | https://galileo.ai/blog/mastering-rag-how-to-select-a-reranking-model | 英語 | Pointwise/Pairwise/Listwiseの分類と選定ガイド |

**FlashRank（軽量・高速・CPU対応）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| FlashRank reranker - LangChain | https://python.langchain.com/docs/integrations/retrievers/flashrank-reranker/ | 英語 | **LangChain公式**。ContextualCompressionRetriever統合 |
| RAG VIII — FlashReranker | https://medium.com/@danushidk507/rag-viii-flashreranker-1afe142592fe | 英語 | 軽量・高速・CPU対応の特徴解説 |

**日本語リランク総合リソース**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| リランキングモデルによるRAGの日本語検索精度の向上 | https://developer.nvidia.com/ja-jp/blog/rag-with-sota-reranking-model-in-japanese/ | 日本語 | **NVIDIA公式**。NV-RerankQAの日本語評価 |
| 日本語最高性能のRerankerをリリース | https://secon.dev/entry/2024/04/02/070000-japanese-reranker-release/ | 日本語 | 日本語特化Rerankerの開発、評価結果 |
| 文書検索におけるリランキングの効果を検証する | https://hironsan.hatenablog.com/entry/information-retrieval-with-reranker | 日本語 | mMiniLM/bge-reranker/Cohereの日本語評価 |

---

## 3. Augmented Generationフェーズ

### 3.1 プロンプトエンジニアリング戦略

#### チャンクの最終選定

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Reranking Explained: Why It Matters for RAG | https://www.chatbase.co/blog/reranking | 英語 | Cross-encoder、LLMリランカー、評価方法の包括的解説 |
| RAG Reranking To Elevate Retrieval Results | https://www.nb-data.com/p/rag-reranking-to-elevate-retrieval | 英語 | LLM-as-a-Judgeによるリランキング、プロンプトテンプレート例 |

#### メタデータの利用

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| OpenAI metadata tagger - LangChain | https://python.langchain.com/docs/integrations/document_transformers/openai_metadata_tagger/ | 英語 | **LangChain公式**。LLMを使った自動メタデータ抽出 |
| Graph-based metadata filtering - LangChain | https://blog.langchain.com/graph-based-metadata-filtering-for-improving-vector-search-in-rag-applications/ | 英語 | Neo4jを使ったグラフベースのメタデータフィルタリング |

#### 【調査結果】その他のプロンプト戦略

**Lost in the Middle問題への対処**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Lost in the Middle（原論文） | https://arxiv.org/abs/2307.03172 | 英語 | コンテキスト中央の情報が無視される問題の実証 |
| Lost in the Middle (TACL 2024) | https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00638/119630/ | 英語 | U字型パフォーマンスカーブの詳細分析 |
| LongLLMLingua: Bye-bye to Middle Loss | https://www.llamaindex.ai/blog/longllmlingua-bye-bye-to-middle-loss-and-save-on-your-rag-costs-via-prompt-compression-54b559b9ddf7 | 英語 | **LlamaIndex公式**。Prompt Compressionによる問題軽減 |

**コンテキスト圧縮（Context Compression）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| LLMLingua - GitHub (Microsoft) | https://github.com/microsoft/LLMLingua | 英語 | **Microsoft公式**。最大20倍圧縮可能なプロンプト圧縮ツール |
| LLMLingua Series Project Page | https://www.llmlingua.com/ | 英語 | LLMLingua/LongLLMLingua/LLMLingua-2の公式ページ |
| How to Cut RAG Costs by 80% Using Prompt Compression | https://towardsdatascience.com/how-to-cut-rag-costs-by-80-using-prompt-compression-877a07c6bedb/ | 英語 | AutoCompressors、Selective Context、LongLLMLinguaの比較 |

**Chain of Thought（CoT）とRAGの組み合わせ**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Chain-of-Thought Prompting - Prompt Engineering Guide | https://www.promptingguide.ai/techniques/cot | 英語 | CoTの基礎からAuto-CoT、Zero-shot CoTまで網羅 |
| RAG + CoT ⇒ Retrieval Augmented Thoughts (RAT) | https://medium.com/@bijit211987/rag-chain-of-thought-retrieval-augmented-thoughts-rat-3d3489517bf0 | 英語 | RAGとCoTを組み合わせた「RAT」手法の詳細 |

**Few-shot Examples in RAG**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Build a RAG App - LangChain | https://python.langchain.com/v0.2/docs/tutorials/rag/ | 英語 | **LangChain公式**。プロンプトテンプレートのカスタマイズ方法 |
| RAG - Prompt Engineering Guide | https://www.promptingguide.ai/techniques/rag | 英語 | RAGとFew-shot学習の組み合わせパターン |

**System Promptの設計**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAGの設計力とは？プロンプトと文脈の最適化 | https://actionbridge.io/ja-JP/llmtutorial/p/llm-rag-chapter6-context-and-prompt-design | 日本語 | System指示、Context、Queryの設計パターン |
| RAGにおけるプロンプト合成の設計パターン | https://actionbridge.io/ja-JP/llmtutorial/p/llm-rag-chapter6-3-prompt-composition-patterns | 日本語 | Retriever出力のプロンプト統合、出力形式誘導 |

**引用・ソース表示の戦略**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| How to get a RAG application to add citations | https://python.langchain.com/v0.2/docs/how_to/qa_citations/ | 英語 | **LangChain公式**。5つの引用手法（Tool-calling等）の比較 |
| Building Trustworthy RAG Systems with In Text Citations | https://haruiz.github.io/blog/improve-rag-systems-reliability-with-citations | 英語 | LangChain/LlamaIndex/Gemini APIの引用実装 |
| Retrieval Augmented Generation with Citations | https://zilliz.com/blog/retrieval-augmented-generation-with-citations | 英語 | LlamaIndex CitationQueryEngineを使った実装 |

**Hallucination防止のためのプロンプト設計**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Three Prompt Engineering Methods to Reduce Hallucinations | https://www.prompthub.us/blog/three-prompt-engineering-methods-to-reduce-hallucinations | 英語 | "According to..."、Step-Back Prompting等の実践的手法 |
| How to Prevent LLM Hallucinations: 5 Proven Strategies | https://www.voiceflow.com/blog/prevent-llm-hallucinations | 英語 | RAG、CoT、RLHF、Guardrailsの組み合わせで96%削減 |
| ChatGPTでハルシネーションを抑制する対策 | https://ai-market.jp/howto/chatgpt-hallucination/ | 日本語 | プロンプト設計、RAG、温度設定の包括的解説 |

**FLARE（Forward-Looking Active REtrieval）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Active Retrieval Augmented Generation（原論文） | https://arxiv.org/abs/2305.06983 | 英語 | 低信頼度トークン検出と動的検索 |
| FLARE / Active RAG | https://learnprompting.org/docs/retrieval_augmented_generation/flare-active-rag | 英語 | FLARE Instruct/FLARE Directの違い |
| Forward-Looking Active REtrieval (FLARE) - DataStax | https://docs.datastax.com/en/ragstack/examples/flare.html | 英語 | LangChain FlareChainを使った実装チュートリアル |

**Self-RAG（自己反省型RAG）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Self-RAG - LangGraph | https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_self_rag/ | 英語 | **LangGraph公式**。自己反省・自己採点の実装 |
| Self-Reflective RAG with LangGraph | https://blog.langchain.com/agentic-rag-with-langgraph/ | 英語 | **LangChain公式**。Self-RAGとCRAGの比較 |
| Advanced RAG Techniques - Pinecone | https://www.pinecone.io/learn/advanced-rag-techniques/ | 英語 | Self-RAG、CRAG、Multi-Queryの統合実装例 |

**Corrective RAG（CRAG）**

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| Corrective RAG (CRAG) - LangGraph | https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_crag/ | 英語 | **LangGraph公式**。Retrieval Evaluator、Web検索補完 |
| Corrective RAG (CRAG) Implementation With LangGraph | https://www.datacamp.com/tutorial/corrective-rag-crag | 英語 | ステップバイステップ実装ガイド |
| Corrective RAG | https://learnprompting.org/docs/retrieval_augmented_generation/corrective-rag | 英語 | 3アクション（Correct/Incorrect/Ambiguous）の解説 |

---

## Azure AI Search関連リソース（横断的）

| タイトル | URL | 言語 | 学べる内容 |
|---------|-----|------|-----------|
| RAG and generative AI - Azure AI Search | https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview | 英語 | **Microsoft公式**。Agentic Retrieval、ハイブリッド検索、セマンティックランキング |
| Quickstart: Generative Search (RAG) | https://learn.microsoft.com/en-us/azure/search/search-get-started-rag | 英語 | Azure AI Search + OpenAIのRAGクイックスタート |
| Build Intelligent RAG For Multimodality | https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/build-intelligent-rag-for-multimodality-and-complex-document/ba-p/4118184 | 英語 | Document Intelligence + LangChain + OpenAIの統合実装 |
| Azure AI Searchの検索手法とスコアリング | https://zenn.dev/headwaters/articles/a2e6be46ed02d2 | 日本語 | 検索手法ごとのスコアリング違い、ハイブリッド+セマンティック検索 |

---

## まとめ：発見された追加手法

### チャンキング戦略の追加手法
- **Late Chunking**（2024年Jina AI発表）：文書全体を先に埋め込み、後でチャンク分割
- **RAPTOR**：再帰的クラスタリングと要約によるツリー構造構築
- **Agentic Chunking**：LLMが動的に分割方法を決定
- **Small-to-Big Retrieval**：小チャンク検索→大チャンク取得

### 検索戦略の追加手法
- **Contextual Retrieval**（Anthropic）：検索エラー67%削減
- **HyDE**：仮説的文書生成によるゼロショット検索改善
- **Multi-Query / Self-Query**：クエリ変換による検索精度向上
- **Graph RAG**：ナレッジグラフを活用した構造的検索

### リランク戦略の追加手法
- **ColBERT**：Late Interactionによる高精度リランキング
- **LLM-based Listwise Reranking**：RankZephyr、RankGPT
- **FlashRank**：軽量・高速・CPU対応

### プロンプト戦略の追加手法
- **Self-RAG / CRAG**：自己反省・修正機能を持つ高度なRAG
- **FLARE**：低信頼度トークン検出と動的検索
- **LLMLingua**：最大20倍のプロンプト圧縮でコスト80%削減
- **Lost in the Middle対策**：重要情報のプロンプト配置最適化

本ガイドは**100以上の教育リソース**を収録しており、日本語リソースも約30件含まれています。公式ドキュメント、学術論文、実践的チュートリアルをバランスよく網羅し、RAGシステム構築の自走学習に最適な内容となっています。