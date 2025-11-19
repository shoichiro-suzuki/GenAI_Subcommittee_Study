# GenAI Subcommittee Study

- このリポジトリはAIコーディングツール活用手法をまとめるためのコンテンツひな形。
- `docs/`配下の各ドキュメントは説明用サンプルであり、実運用の要件や仕様を直接反映していない。
- 具体的な手順や設計を記載する際はサンプルを差し替え、最新の知見と同期させること。

## ディレクトリ構成

```
.
├── docs/ # 中身はサンプル
│   ├── 要件定義書.md
│   ├── データスキーマ定義書.md
│   ├── アーキテクチャ設計書.md
│   ├── CLI仕様書.md
│   └── 開発TODO.md
│
├── .gitignore # Git対象外ファイル一覧
│
│   # MCP設定ファイル
├── .claude\settings.local.json # Claude
├── .mcp.json # MCP設定ファイル：Claude
│
│   # エージェント指示ファイル
├── AGENTS.md # Codex
├── CLAUDE.md # Claude
├── .github\copilot-instructions.md # Github Copilot
│
├── AI支援開発の提案.pdf
├── Claudeプロンプト経緯.md
├── Git & GitHub ガイド.pdf
└── README.md # メイン説明書
```
