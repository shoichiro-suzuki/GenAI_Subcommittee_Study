# GUIデザイン書

## 1. UI概要

### 1.1 GUIフレームワーク
- **tkinter**: Python標準ライブラリ（外部依存なし）
- **配布**: PyInstallerで単一EXEに包含

### 1.2 ウィンドウ仕様
- **サイズ**: 600x400px（固定）
- **リサイズ**: 不可
- **タイトル**: "SimpleTodo"

## 2. レイアウト設計

### 2.1 全体構成
```
┌─────────────────────────────────────────┐
│ SimpleTodo                         [×]  │
├─────────────────────────────────────────┤
│ ┌─────────────────────────────────────┐ │
│ │ 新しいタスク: [________________] [追加]│ │
│ └─────────────────────────────────────┘ │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │ ID │タイトル        │状態  │作成日時│ │
│ ├───┼───────────────┼─────┼────────┤ │
│ │ 1  │サンプルタスク  │未完了│01/15...│ │
│ │ 2  │完了したタスク  │完了  │01/15...│ │
│ │    │                │      │        │ │
│ │    │   (スクロール可能領域)   │        │ │
│ │    │                │      │        │ │
│ └─────────────────────────────────────┘ │
│                                         │
│     [完了/未完了切替]  [削除]           │
└─────────────────────────────────────────┘
```

### 2.2 コンポーネント配置
| エリア | コンポーネント | 説明 |
|-------|-------------|------|
| 上部 | Entry + Button | Todo追加フォーム |
| 中央 | Treeview + Scrollbar | Todo一覧表示 |
| 下部 | Button × 2 | 操作ボタン |

## 3. コンポーネント詳細

### 3.1 Todo追加エリア
```python
# Entryウィジェット
- width: 50文字
- placeholder: "新しいタスクを入力..."
- 最大長: 200文字

# 追加ボタン
- text: "追加"
- width: 10
- command: add_todo_handler()
- Enterキーでも実行可能
```

**動作**:
1. Entryにテキスト入力
2. [追加]ボタンまたはEnterキー押下
3. バリデーション（1-200文字）
4. `todo_service.add_todo()`呼び出し
5. 一覧を再描画
6. Entryをクリア

### 3.2 Todo一覧エリア
```python
# Treeviewウィジェット
columns: ["ID", "タイトル", "状態", "作成日時"]
height: 15行
selectmode: browse（単一選択）

# 列幅設定
- ID: 50px
- タイトル: 250px
- 状態: 80px
- 作成日時: 150px
```

**表示ルール**:
- 完了済みは背景色変更（薄いグレー）
- 選択行はハイライト
- ダブルクリックで完了/未完了切替

### 3.3 操作ボタンエリア
```python
# 完了/未完了切替ボタン
- text: "完了/未完了切替"
- width: 15
- command: toggle_todo_handler()
- 選択行なし時は無効化

# 削除ボタン
- text: "削除"
- width: 10
- command: delete_todo_handler()
- 選択行なし時は無効化
```

## 4. イベントハンドリング

### 4.1 Todo追加
```python
def add_todo_handler():
    title = entry.get()
    if not title:
        messagebox.showwarning("警告", "タスクを入力してください")
        return
    try:
        todo_service.add_todo(title)
        refresh_list()
        entry.delete(0, END)
    except ValueError as e:
        messagebox.showerror("エラー", str(e))
```

### 4.2 完了切替
```python
def toggle_todo_handler():
    selected = tree.selection()
    if not selected:
        return
    item = tree.item(selected[0])
    todo_id = item['values'][0]
    try:
        todo_service.toggle_todo(todo_id)
        refresh_list()
    except ValueError as e:
        messagebox.showerror("エラー", str(e))
```

### 4.3 削除
```python
def delete_todo_handler():
    selected = tree.selection()
    if not selected:
        return
    item = tree.item(selected[0])
    todo_id = item['values'][0]
    if messagebox.askyesno("確認", "本当に削除しますか？"):
        try:
            todo_service.delete_todo(todo_id)
            refresh_list()
        except ValueError as e:
            messagebox.showerror("エラー", str(e))
```

### 4.4 ダブルクリック
```python
def on_double_click(event):
    toggle_todo_handler()

tree.bind("<Double-1>", on_double_click)
```

## 5. データ表示ロジック

### 5.1 一覧更新
```python
def refresh_list():
    # 既存項目をクリア
    tree.delete(*tree.get_children())

    # データ取得
    todos = todo_service.list_todos()

    # Treeviewに挿入
    for todo in todos:
        status = "完了" if todo.completed else "未完了"
        created = format_datetime(todo.created_at)

        tree.insert("", END, values=(
            todo.id,
            todo.title,
            status,
            created
        ), tags=("completed" if todo.completed else ""))

    # 完了済みタグのスタイル設定
    tree.tag_configure("completed", background="#f0f0f0")
```

### 5.2 日時フォーマット
```python
def format_datetime(iso_str: str) -> str:
    dt = datetime.fromisoformat(iso_str)
    return dt.strftime("%m/%d %H:%M")
```

## 6. エラー表示

### 6.1 メッセージボックス
```python
# 警告
messagebox.showwarning("警告", "タスクを入力してください")

# エラー
messagebox.showerror("エラー", "タイトルは1-200文字で入力してください")

# 確認ダイアログ
result = messagebox.askyesno("確認", "本当に削除しますか？")
```

### 6.2 主要エラーメッセージ
| エラー | メッセージ |
|-------|----------|
| 空タイトル | "タスクを入力してください" |
| タイトル超過 | "タイトルは200文字以内で入力してください" |
| ID不存在 | "指定されたタスクが見つかりません" |
| データ破損 | "データファイルの読み込みに失敗しました" |

## 7. スタイル設定

### 7.1 色定義
```python
COLOR_BG = "#ffffff"           # 背景色
COLOR_COMPLETED = "#f0f0f0"    # 完了済み背景
COLOR_SELECTED = "#0078d7"     # 選択色
COLOR_TEXT = "#000000"         # テキスト色
```

### 7.2 フォント設定
```python
FONT_DEFAULT = ("Yu Gothic UI", 10)
FONT_HEADER = ("Yu Gothic UI", 10, "bold")
FONT_ENTRY = ("Yu Gothic UI", 11)
```

## 8. ウィンドウ初期化

### 8.1 起動時処理
```python
def main():
    root = Tk()
    root.title("SimpleTodo")
    root.geometry("600x400")
    root.resizable(False, False)

    # UI構築
    build_ui(root)

    # 初期データ読み込み
    refresh_list()

    # メインループ
    root.mainloop()
```

### 8.2 ウィンドウクローズ
```python
def on_closing():
    # 保存確認不要（自動保存済み）
    root.destroy()

root.protocol("WM_DELETE_WINDOW", on_closing)
```

## 9. キーボードショートカット

| キー | 動作 |
|-----|------|
| Enter（Entry内） | Todo追加 |
| Delete | 選択行削除（確認あり） |
| Space | 完了/未完了切替 |
| Esc | 選択解除 |

実装例:
```python
root.bind("<Delete>", lambda e: delete_todo_handler())
root.bind("<space>", lambda e: toggle_todo_handler())
root.bind("<Escape>", lambda e: tree.selection_remove(tree.selection()))
```

## 10. アクセシビリティ

### 10.1 タブオーダー
1. Entry（タスク入力）
2. 追加ボタン
3. Treeview（一覧）
4. 完了/未完了切替ボタン
5. 削除ボタン

### 10.2 フォーカス初期位置
起動時: Entry（タスク入力欄）にフォーカス

```python
entry.focus_set()
```
