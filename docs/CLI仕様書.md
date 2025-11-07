# CLI仕様書

## 1. 基本コマンド構成

### 1.1 コマンド形式
```bash
SimpleTodo <command> [options]
```

### 1.2 コマンド一覧
| コマンド | 説明 | エイリアス |
|---------|------|----------|
| `add` | Todoを追加 | `a` |
| `list` | Todo一覧を表示 | `ls`, `l` |
| `toggle` | 完了状態を切り替え | `t` |
| `delete` | Todoを削除 | `del`, `d` |
| `help` | ヘルプを表示 | `-h`, `--help` |

## 2. コマンド詳細

### 2.1 add - Todo追加

#### 書式
```bash
SimpleTodo add <title>
SimpleTodo add "タスクのタイトル"
```

#### 引数
- `<title>`: Todoのタイトル（必須、1〜200文字）

#### 実行例
```bash
# 基本的な追加
SimpleTodo add "買い物に行く"

# エイリアス使用
SimpleTodo a "レポートを書く"
```

#### 出力
```
✓ Todo追加: 買い物に行く (ID: 1)
```

#### エラー
```bash
# タイトルなし
SimpleTodo add
→ エラー: タイトルを指定してください

# 200文字超過
SimpleTodo add "あああ..." (201文字)
→ エラー: タイトルは200文字以内で指定してください
```

---

### 2.2 list - Todo一覧表示

#### 書式
```bash
SimpleTodo list [--all]
SimpleTodo list [-a]
```

#### オプション
| オプション | 説明 | デフォルト |
|----------|------|----------|
| `--all`, `-a` | 完了済みも含めて表示 | 未完了のみ |

#### 実行例
```bash
# 未完了のみ表示
SimpleTodo list

# すべて表示
SimpleTodo list --all
SimpleTodo ls -a
```

#### 出力形式
```
ID  状態  タイトル
─────────────────────────────
1   [ ]   買い物に行く
2   [ ]   レポートを書く
3   [✓]   メールを返信
─────────────────────────────
未完了: 2件 / 完了: 1件 / 合計: 3件
```

**表示ルール**:
- 状態: `[ ]`（未完了）、`[✓]`（完了）
- ID順にソート
- 空の場合: `Todoはまだありません`

---

### 2.3 toggle - 完了状態切り替え

#### 書式
```bash
SimpleTodo toggle <id>
```

#### 引数
- `<id>`: TodoのID（必須、正の整数）

#### 実行例
```bash
# 未完了 → 完了
SimpleTodo toggle 1
→ ✓ Todo完了: 買い物に行く (ID: 1)

# 完了 → 未完了
SimpleTodo toggle 1
→ ○ Todo未完了に戻す: 買い物に行く (ID: 1)
```

#### エラー
```bash
# 存在しないID
SimpleTodo toggle 999
→ エラー: ID 999 のTodoが見つかりません

# ID未指定
SimpleTodo toggle
→ エラー: IDを指定してください
```

---

### 2.4 delete - Todo削除

#### 書式
```bash
SimpleTodo delete <id> [--force]
```

#### 引数・オプション
- `<id>`: TodoのID（必須、正の整数）
- `--force`, `-f`: 確認なしで削除

#### 実行例
```bash
# 確認あり削除（デフォルト）
SimpleTodo delete 1
→ 本当に削除しますか? [y/N]: y
→ ✓ Todo削除: 買い物に行く (ID: 1)

# 確認なし削除
SimpleTodo del 2 --force
→ ✓ Todo削除: レポートを書く (ID: 2)
```

#### エラー
```bash
# 存在しないID
SimpleTodo delete 999
→ エラー: ID 999 のTodoが見つかりません

# 削除キャンセル
SimpleTodo delete 1
→ 本当に削除しますか? [y/N]: n
→ 削除をキャンセルしました
```

---

### 2.5 help - ヘルプ表示

#### 書式
```bash
SimpleTodo help
SimpleTodo --help
SimpleTodo -h
```

#### 出力
```
SimpleTodo - シンプルなTodo管理アプリ

使い方:
  SimpleTodo <command> [options]

コマンド:
  add <title>          Todoを追加
  list [--all]         Todo一覧を表示（未完了のみ or すべて）
  toggle <id>          完了状態を切り替え
  delete <id> [--force] Todoを削除

エイリアス:
  a, ls, l, t, del, d

例:
  SimpleTodo add "買い物に行く"
  SimpleTodo list --all
  SimpleTodo toggle 1
  SimpleTodo delete 2 --force
```

## 3. 共通仕様

### 3.1 終了コード
| コード | 意味 |
|-------|------|
| 0 | 正常終了 |
| 1 | エラー（引数不正、ID不存在など） |
| 2 | データファイルエラー |

### 3.2 エラーメッセージ形式
```
エラー: <エラー内容>
```

**標準エラー出力に出力**

### 3.3 文字エンコーディング
- **入力**: UTF-8
- **出力**: UTF-8（Windows環境ではコンソールのエンコーディングに自動対応）

## 4. 対話的操作

### 4.1 引数なし起動
```bash
SimpleTodo
```

**動作**: ヘルプを表示

### 4.2 不正なコマンド
```bash
SimpleTodo unknown
```

**動作**: エラーメッセージ + ヘルプ表示

## 5. データファイル操作

### 5.1 初回起動
```bash
SimpleTodo list
```

**動作**:
1. `todos.json`が存在しない場合、自動生成
2. 初期状態（空のTodoリスト）で起動

### 5.2 ファイル破損時
**動作**:
1. `todos.json.backup`にバックアップ
2. 新しい`todos.json`を初期状態で生成
3. 警告メッセージ表示
```
警告: データファイルが破損していたため、バックアップを作成し初期化しました
バックアップ: todos.json.backup
```

## 6. 表示形式の詳細

### 6.1 日時表示
不要（内部管理のみ、ユーザーには非表示）

### 6.2 タイトルの切り詰め
- 50文字を超える場合: `...`で省略
- 例: `これは非常に長いタイトルで50文字を超えてい...`

### 6.3 色表示
初期バージョンでは色なし（将来拡張可能）

## 7. 将来拡張予定

以下の機能は将来バージョンで実装予定：
- `search <keyword>`: キーワード検索
- `edit <id> <new_title>`: タイトル編集
- `priority <id> <level>`: 優先度設定
- `export <format>`: データエクスポート（CSV, Markdown）
