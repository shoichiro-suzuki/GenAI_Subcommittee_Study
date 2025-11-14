# 2. OpenAI Function Calling

## 2.1 対応モデル

### GPT-5シリーズ（2025年8月リリース、最新）
- `gpt-5`（レギュラー）
- `gpt-5-mini`（軽量版）
- `gpt-5-nano`（超軽量版）

**特徴**：
- 400Kトークンコンテキスト、128K出力制限
- 4段階の推論レベル選択可能
- カスタムツール対応（JSONなしでプレーンテキスト送信）
- ツール呼び出し精度が大幅向上（τ2-benchで96.7%）
- 並列・シーケンシャルツール呼び出しの高度な制御
- 価格：$1.25/M入力、$10/M出力（GPT-4oの半額）

### GPT-5.1シリーズ（2025年11月リリース、最新）
- `gpt-5.1-instant`：温かく会話的、適応型推論、高速応答
- `gpt-5.1-thinking`：複雑な思考タスク用、深い推論

**特徴**：
- 簡単タスクでGPT-5より57%高速
- 複雑タスクでGPT-5より71%長く思考可能
- 会話の自然さとフレンドリーさが改善

### GPT-4oシリーズ（後方互換）
- `gpt-4o`
- 高速かつ高精度、マルチモーダル対応
- 一部ユーザーに人気（温かいトーン）

### GPT-4シリーズ（後方互換）
- `gpt-4-0613` 以降
- 高精度な関数選択・引数生成

### GPT-3.5シリーズ（後方互換）
- `gpt-3.5-turbo-0613` 以降
- Function calling対応の最初のモデル、コスト効率が良い

### モデル選択の指針（2025年版）
- **最新機能・最高性能**: GPT-5.1-instant（推奨）
- **複雑な推論タスク**: GPT-5.1-thinking
- **コスト重視・軽量タスク**: gpt-5-mini / gpt-5-nano
- **温かいトーン重視**: gpt-4o（一部ユーザーに好評）
- **レガシーシステム**: GPT-3.5-turbo（非推奨）

## 2.2 API構造

### 基本的なAPI呼び出し（従来のJSONツール）

```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-5.1-instant",  # 最新モデル推奨
    messages=[
        {"role": "user", "content": "東京の天気を教えて"}
    ],
    tools=[
        {
            "type": "function",
            "function": {
                "name": "get_weather",
                "description": "指定された場所の天気情報を取得",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "都市名"
                        }
                    },
                    "required": ["location"]
                }
            }
        }
    ],
    verbosity="medium"  # GPT-5の新パラメータ：low/medium/high
)
```

### カスタムツール（GPT-5の新機能）

GPT-5以降では、JSON形式ではなくプレーンテキストでツールを呼び出せる：

```python
response = openai.ChatCompletion.create(
    model="gpt-5",
    messages=[
        {"role": "user", "content": "データベースから売上データを抽出して"}
    ],
    tools=[
        {
            "type": "custom",  # JSON不要のカスタムツール
            "custom": {
                "name": "execute_sql",
                "description": "SQLクエリを実行してデータベースから情報を取得",
                "input_type": "text/sql"  # プレーンテキスト形式を指定
            }
        }
    ]
)

# GPT-5がSQLクエリをプレーンテキストで返す
# 例: "SELECT * FROM sales WHERE date > '2024-01-01'"
```

**カスタムツールのメリット**：
- Python、SQL、シェルコマンドなどをJSON化せずに直接送信
- 複雑なコードのエスケープが不要
- より自然な形式でツール呼び出しが可能

### 主要パラメータ

#### `tools` パラメータ
- 利用可能な関数のリストを定義
- `type: "function"` - 従来のJSONベースツール
- `type: "custom"` - GPT-5の新機能（プレーンテキストツール）

#### `tool_choice` パラメータ
関数呼び出しの制御方法を指定：

| 値 | 動作 |
|---|---|
| `"auto"` (デフォルト) | LLMが自動判断 |
| `"required"` | 必ず何かの関数を呼び出す |
| `"none"` | 関数を呼び出さない |
| `{"type": "function", "function": {"name": "関数名"}}` | 特定の関数を強制 |

**使用例**：
```python
# 必ず関数を呼び出す
tool_choice="required"

# 特定の関数を強制
tool_choice={
    "type": "function",
    "function": {"name": "get_weather"}
}
```

#### `verbosity` パラメータ（GPT-5以降）
レスポンスの詳細度を制御：

| 値 | 動作 |
|---|---|
| `"low"` | 簡潔な回答 |
| `"medium"` (デフォルト) | 標準的な詳細度 |
| `"high"` | 詳細な説明 |

```python
response = openai.ChatCompletion.create(
    model="gpt-5.1-instant",
    verbosity="low"  # 簡潔な回答を要求
)
```

#### `reasoning_effort` パラメータ（GPT-5.1-thinking）
推論の深さを制御：

| 値 | 動作 |
|---|---|
| `"minimal"` | 最小限の推論（高速） |
| `"low"` | 軽い推論 |
| `"medium"` (デフォルト) | 標準的な推論 |
| `"high"` | 深い推論（複雑なタスク向け） |

```python
response = openai.ChatCompletion.create(
    model="gpt-5.1-thinking",
    reasoning_effort="high"  # 複雑な問題に深く推論
)
```

### レスポンス形式

#### 関数呼び出しが必要な場合

```json
{
  "choices": [{
    "message": {
      "role": "assistant",
      "content": null,
      "tool_calls": [{
        "id": "call_abc123",
        "type": "function",
        "function": {
          "name": "get_weather",
          "arguments": "{\"location\": \"東京\"}"
        }
      }]
    },
    "finish_reason": "tool_calls"
  }]
}
```

#### 重要なフィールド
- `finish_reason: "tool_calls"`: 関数呼び出しが必要
- `tool_calls`: 呼び出す関数の配列
- `tool_calls[].id`: ツール呼び出しのID（結果返却時に使用）
- `function.arguments`: JSON文字列形式の引数

---

## 参考情報

### 公式ドキュメント（最新）
- [GPT-5 Models - OpenAI API](https://platform.openai.com/docs/models/gpt-5)
  - GPT-5シリーズの公式ドキュメント

- [GPT-5.1: A smarter, more conversational ChatGPT - OpenAI](https://openai.com/index/gpt-5-1/)
  - GPT-5.1の公式発表

- [GPT-5 New Params and Tools - OpenAI Cookbook](https://cookbook.openai.com/examples/gpt-5/gpt-5_new_params_and_tools)
  - GPT-5のカスタムツール、新パラメータの実装例

- [Function calling - OpenAI API](https://platform.openai.com/docs/guides/function-calling)
  - Function callingの基本ガイド

- [OpenAI Cookbook - How to call functions](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb)
  - 公式のJupyter Notebook実装例

### 日本語最新情報（2025年）
- [ChatGPTに｢温かい会話｣が帰ってきた。OpenAIがGPT-5.1をリリース - GIZMODO](https://www.gizmodo.jp/2025/11/openai_chatgpt_gpt_5_1_released.html)
  - GPT-5.1の日本語解説

- [GPT-5 が遂に登場！OpenAI最新AIモデル - スクーティー](https://blog.scuti.jp/gpt-5-comprehensive-guide-performance-pricing-features/)
  - GPT-5の性能、価格、機能の包括的ガイド

- [【GPT-5.1】性能・親しみやすさがアップしたOpenAI最新モデル - WEEL](https://weel.co.jp/media/tech/openai-gpt-5-1/)
  - GPT-5.1の詳細解説と比較検証

### 日本語チュートリアル（基礎）
- [OpenAIがChatGPTを更にパワーアップ！「Function Calling」の使い方とは？ - Qiita](https://qiita.com/windows222/items/bc2c25b3e7526d8926ec)
  - 天気APIを使った実装例

- [ChatGPTの新機能！Function callingでかわいいお天気お姉さんつくーる - Qiita](https://qiita.com/SatoshiGachiFujimoto/items/f55b4187c9f681fe069a)
  - One Call API連携の実例

- [OpenAIのChat APIに追加されたFunction callingを使ってみる - 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2023/06/14/gpt-function-calling-intro/)
  - 基礎から実装までの丁寧な解説
