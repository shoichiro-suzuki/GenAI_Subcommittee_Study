# 2. OpenAI Function Calling

## 2.1 対応モデル

### GPT-3.5シリーズ
- `gpt-3.5-turbo-0613` 以降
- Function calling対応の最初のモデル
- コスト効率が良い

### GPT-4シリーズ
- `gpt-4-0613` 以降
- より高精度な関数選択・引数生成
- 複雑な処理に適している

### GPT-4oシリーズ
- `gpt-4o` (最新)
- 高速かつ高精度
- マルチモーダル対応

### モデル選択の指針
- **シンプルな処理**: GPT-3.5-turbo
- **複雑な判断が必要**: GPT-4
- **最新機能・高速性重視**: GPT-4o

## 2.2 API構造

### 基本的なAPI呼び出し

```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-4o",
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
    ]
)
```

### 主要パラメータ

#### `tools` パラメータ
- 利用可能な関数のリストを定義
- 各関数は `type: "function"` を指定
- `function` オブジェクトで詳細を記述

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

### 公式ドキュメント
- [Function calling - OpenAI API](https://platform.openai.com/docs/guides/function-calling)
  - 公式ガイド、最新の仕様

- [OpenAI Cookbook - How to call functions](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb)
  - 公式のJupyter Notebook実装例

### 日本語チュートリアル
- [OpenAIがChatGPTを更にパワーアップ！「Function Calling」の使い方とは？ - Qiita](https://qiita.com/windows222/items/bc2c25b3e7526d8926ec)
  - 天気APIを使った実装例

- [ChatGPTの新機能！Function callingでかわいいお天気お姉さんつくーる - Qiita](https://qiita.com/SatoshiGachiFujimoto/items/f55b4187c9f681fe069a)
  - One Call API連携の実例

- [OpenAIのChat APIに追加されたFunction callingを使ってみる - 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2023/06/14/gpt-function-calling-intro/)
  - 基礎から実装までの丁寧な解説
