# 開発TODO

## Phase 1: 基盤実装 🔨

### 1.1 プロジェクト初期設定
- [ ] `services/`ディレクトリ作成
- [ ] `tests/`ディレクトリ作成
- [ ] `requirements.txt`作成
- [ ] `.gitignore`更新（build/, dist/, *.spec追加）

### 1.2 データモデル実装
- [ ] `services/models.py`作成
  - [ ] `Todo`データクラス実装
  - [ ] `TodoDatabase`データクラス実装
  - [ ] 型ヒント完備

### 1.3 データ管理層実装
- [ ] `services/data_manager.py`作成
  - [ ] `load_data()`実装
  - [ ] `save_data()`実装
  - [ ] `init_data()`実装
  - [ ] `backup_data()`実装
  - [ ] JSON破損検知・復旧ロジック

## Phase 2: ビジネスロジック実装 ⚙️

### 2.1 Todoサービス実装
- [ ] `services/todo_service.py`作成
  - [ ] `add_todo(title)`実装
  - [ ] `list_todos()`実装
  - [ ] `toggle_todo(id)`実装
  - [ ] `delete_todo(id)`実装
  - [ ] バリデーション実装
    - [ ] タイトル長チェック（1〜200文字）
    - [ ] ID存在確認
    - [ ] 空文字列チェック

## Phase 3: CLI実装 🖥️

### 3.1 CLIインターフェース
- [ ] `services/main.py`作成
  - [ ] argparse設定
    - [ ] `add`コマンド + エイリアス
    - [ ] `list`コマンド + エイリアス
    - [ ] `toggle`コマンド + エイリアス
    - [ ] `delete`コマンド + エイリアス
  - [ ] `display_todos()`実装（テーブル形式表示）
  - [ ] エラーメッセージ表示
  - [ ] ヘルプ表示

### 3.2 対話的機能
- [ ] `delete`コマンドの確認プロンプト実装
- [ ] `--force`オプション実装

## Phase 4: テスト実装 🧪

### 4.1 ユニットテスト
- [ ] `tests/test_models.py`作成
  - [ ] Todoデータクラステスト
  - [ ] TodoDatabaseデータクラステスト
- [ ] `tests/test_data_manager.py`作成
  - [ ] load_data()テスト
  - [ ] save_data()テスト
  - [ ] init_data()テスト
  - [ ] JSON破損時のテスト
- [ ] `tests/test_todo_service.py`作成
  - [ ] add_todo()テスト
  - [ ] toggle_todo()テスト
  - [ ] delete_todo()テスト
  - [ ] バリデーションテスト

### 4.2 統合テスト
- [ ] CLI操作の統合テスト
- [ ] エラーシナリオテスト

## Phase 5: ビルド・リリース準備 📦

### 5.1 ビルド設定
- [ ] `build.spec`作成
  - [ ] PyInstallerオプション設定
  - [ ] アイコン設定（オプション）
  - [ ] データファイル除外設定
- [ ] ビルドスクリプト作成

### 5.2 動作確認
- [ ] Windows環境でEXE実行テスト
- [ ] 初回起動テスト（JSON自動生成）
- [ ] 全コマンド動作確認
- [ ] エラーハンドリング確認

### 5.3 ドキュメント最終確認
- [ ] README.md更新
  - [ ] インストール方法
  - [ ] 使用方法
  - [ ] ビルド方法
- [ ] ドキュメント整合性確認

## Phase 6: 品質向上（オプション） ✨

### 6.1 コード品質
- [ ] 型チェック（mypy）
- [ ] リンター（flake8, pylint）
- [ ] フォーマッター（black）

### 6.2 パフォーマンス
- [ ] 起動時間計測
- [ ] 大量データでの動作確認

### 6.3 ユーザビリティ
- [ ] エラーメッセージの改善
- [ ] ヘルプメッセージの充実

## 優先順位

### P0（最優先）
Phase 1, 2, 3の全項目

### P1（高優先）
Phase 4.1, 5.1, 5.2

### P2（通常）
Phase 4.2, 5.3

### P3（低優先・オプション）
Phase 6の全項目

## マイルストーン

| マイルストーン | 完了条件 | 期限目安 |
|-------------|---------|---------|
| M1: 基盤完成 | Phase 1完了 | - |
| M2: コア機能完成 | Phase 2完了 | - |
| M3: CLI完成 | Phase 3完了 | - |
| M4: テスト完成 | Phase 4完了 | - |
| M5: リリース可能 | Phase 5完了 | - |

## 進捗管理

- [ ] Phase 1: 基盤実装
- [ ] Phase 2: ビジネスロジック
- [ ] Phase 3: CLI実装
- [ ] Phase 4: テスト実装
- [ ] Phase 5: ビルド・リリース準備
- [ ] Phase 6: 品質向上（オプション）
