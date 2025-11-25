# Azure FunctionsでカスタムMCPサーバーを作成してClaudeに接続する

---

以下は、Azure Functionsを使ったカスタムMCPの作成方法を説明します。
Azure FunctionのMCP拡張機能を使って、MCPとしての仕組みをAzure Function側にお任せしています（02_技術仕様で説明したほぼすべての内容を自動調整）。
但し、自動の代わりに「10_MCPの4要素」で述べた `Tools` の機能のみを提供しています。

---

## 準備
1. `Azure Functions Core Tools` のインストール（未インストールのとき）
    ```bash
    # npm をインストールした状態でターミナル実行
    npm install -g azure-functions-core-tools@4
    ```
2. 拡張機能「Azure Functions」を入れる<br>
    <img src="./images/Functions拡張.png" alt="代替テキスト" width="700">

---

## 新しいプロジェクトを作る
- 新規プロジェクトを作りたい場所をVScodeで開く（作りたいフォルダのひとつ上の階層）
- 以下をターミナル実行
    ```bash
    func init <プロジェクト名>
    ```
    <img src="./images/001.png" alt="代替テキスト" width="600">
- Pythonを選ぶ<br>
    <img src="./images/002.png" alt="代替テキスト" width="600">
- 作成されたフォルダを開き直す
- requirements.txt を使って環境作成（仮想環境使うならアクティブ化する）
- 中身確認：以下が自動で作られたはず<br>
    <img src="./images/010.png" alt="代替テキスト" width="400">

---

## host.json 修正
- `host.json` を修正： `Experimental` を追記<br>
    <img src="./images/101.png" alt="代替テキスト" width="500"> 

---

## function_app.py 修正
- AIコーディングに指示：<br>
**プロンプトの要点**
    ```
    ## 前提
    Azure functions のMCP拡張を使ってカスタムMCPサーバーを作成します。
    - Python v2 の FunctionApp を使い、各 MCP ツールは 1 関数 = 1 ツール として公開
    - 次の構文で MCP ツールを定義する：
        - @app.mcp_tool_trigger(arg_name="context", tool_name="...", description="...", tool_properties="...")
        - 例：
            ```
            import azure.functions as func

            # Azure Functions アプリの初期化
            app = func.FunctionApp(http_auth_level=func.AuthLevel.FUNCTION)

            # 疎通確認用のシンプルなMCPツール
            @app.mcp_tool_trigger(
                arg_name="context",
                tool_name="health_check",
                description="MCPサーバーの疎通確認",
                tool_properties="[]"
            )
            def health_check(context) -> str:
                """
                MCPサーバーの疎通確認用エンドポイント

                Returns:
                    str: ステータス情報のJSON文字列
                """
                try:
                    return json.dumps({
                        "status": "healthy",
                        "message": "MCPサーバーは正常に動作しています",
                        "service": "Azure AI Search Hybrid Search MCP Server",
                        "version": "1.0.0"
                    }, ensure_ascii=False)

                except Exception as e:
                    logger.error(f"ヘルスチェックエラー: {str(e)}")
                    return json.dumps({
                        "status": "error",
                        "message": str(e)
                    }, ensure_ascii=False)
            ```

    ## 要求機能（ツール）
    - 欲しい機能その１
    - 欲しい機能その２
    - ...
    ```

---

## Github登録
- 新しいリポジトリに登録しておく

---

## Azure Functions リソース作成
- `Functions` で検索<br>
    <img src="./images/200.png" alt="代替テキスト" width="500">
- `フレックス従量課金` を選択<br>
    <img src="./images/201.png" alt="代替テキスト" width="500"> 
- 設定を進む（説明は省略）
- ストレージには関連ファイルが保存される。既存のBlobストレージでもよいが、自分で分かり易い名前を付けることをおすすめ。<br>
    <img src="./images/202.png" alt="代替テキスト" width="600"> 
- デプロイ設定はGithubを選択：新しく作成したリポジトリを指定<br>
    <img src="./images/203.png" alt="代替テキスト" width="800"> 
- FunctionsにGithub連携した時点で自動的にデプロイが開始される。
    しかし、自動的に作成される設定ファイル（※）が原因で失敗する<br>
    <img src="./images/204.png" alt="代替テキスト" width="800"> <br>
    ※　自動デプロイのときに `.github/workflows` が作成される。<br>
        <img src="./images/205.png" alt="代替テキスト" width="800"> 
- `.github/workflows` を直すために、VScodeに `fetch` する<br>
    <img src="./images/206.png" alt="代替テキスト" width="500"> 
- `.github/workflows` の中に `*.yml` があるので以下のように直す
    末尾に以下を追加
    ```yml
    sku: 'flexconsumption'
    remote-build: 'true'
    ```
    <img src="./images/207.png" alt="代替テキスト" width="1000"> 
- 変更をコミットしてGithub更新 → 自動的にAzure Functionも再デプロイ開始<br>
    <img src="./images/208.png" alt="代替テキスト" width="800"> 
- 成功して完了したところ<br>
    <img src="./images/209.png" alt="代替テキスト" width="800"> 

---

## ClaudeにMCPを接続
> **注意**<br>
> 　下記の接続方法は認証がなく、シークレットをURLに直接埋め込んだ **とりあえず接続できる** 方法です。<br>
> 　接続URLがそのまま漏れると、部外者でもカスタムMCPに接続できます。<br>
> 　認証込みの仕組みが確立するまでは、開発者＋信用できる範囲までの公開に留めてください。<br>
> 　なお、Webアプリ化したシステムからFunctionsのMCPにつなぐ場合は、URLを環境変数で管理すれば問題ありません。


- Azureポータル ＞ Azure Functions ＞ 概要 ＞ 既定のドメイン　をコピー<br>
    <img src="./images/300.png" alt="代替テキスト" width="1000"> 
- Azure Functions ＞ 関数 ＞ アプリキー ＞ mcp_extension　をコピー<br>
    <img src="./images/301.png" alt="代替テキスト" width="800"> 
- FunctionsのMCP拡張にアクセスするURLを作る
    ```
    https://<既定のドメイン>/runtime/webhooks/mcp/sse?code=<mcp_extensionの値>
    ```
- Claude ＞ 設定 ＞ コネクタ ＞ カスタムコネクタを追加
- コネクタの名前とURLを記入して追加<br>
    <img src="./images/302.png" alt="代替テキスト" width="600"> 
- 接続成功を確認したら、新規チャットでコネクタを有効化<br>
    <img src="./images/303.png" alt="代替テキスト" width="600"> 
- カスタムコネクタを使ってみた<br>
    <img src="./images/304.png" alt="代替テキスト" width="800"> 

### CtatGPTの場合
- ChatGPT ＞ 設定 ＞ アプリとコネクタ ＞ 一番下までスクロール ＞ 高度な設定 ＞ 開発者モードをオン
- ChatGPT ＞ 設定 ＞ アプリとコネクタ ＞ 右上の`作成する`
- 名前、説明、URLを記入。認証は`認証なし`。リスクへの警告にチェックして作成。