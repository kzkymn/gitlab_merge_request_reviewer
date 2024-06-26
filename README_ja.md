# code_review_request_inspector

`code_review_request_inspector`は、GitLabのマージリクエストおよびGitHubのプルリクエストの内容理解やコードレビューをChatGPTに行わせるツールです。このツールは、OpenAI APIまたはAzure OpenAIのAPIを使用して、コードの変更点に対するフィードバックを提供します。

## 実行方法

### Python環境の準備

1. **Pythonのインストール**:
    - 本ツールはPython 3.11系で動作確認されています。Pythonがインストールされていない場合は、[公式サイト](https://www.python.org/downloads/)からインストールしてください。

2. **依存パッケージのインストール**:
    - 必要なパッケージは`requirements.txt`に記載されています。venvなどで本ツール用のPython 3.11環境を構築してから、以下のコマンドを実行してパッケージをインストールしてください。

    ```sh
    pip install -r requirements.txt
    ```

### config.iniの設定

`config.ini`ファイルは、本ツールの動作に必要な設定を行うためのファイルです。以下の内容を参考に、適切な値を設定してください。

```ini:config.ini
[service]
provider = github  # 使用するサービスプロバイダーを指定します。github または gitlab を指定します。

[github]
url = https://api.github.com  # GitHub APIのURLを指定します。
token = YOUR_GITHUB_TOKEN  # GitHubのAPIトークンを指定します。GitHubの個人設定から取得できます。
owner = REPO_OWNER  # リポジトリの所有者の名前を指定します。
repo = REPO_NAME  # 対象のリポジトリ名を指定します。
pull_request_number = PULL_REQUEST_NUMBER  # 対象のプルリクエスト番号を指定します。

[gitlab]
url = https://gitlab.com/  # GitLabのURLを指定します。通常はhttps://gitlab.com/です。
private_token = your_private_token  # GitLabのプライベートトークンを指定します。GitLabのユーザー設定から取得できます。
project_id = 11111111  # 対象プロジェクトのIDを指定します。プロジェクトの設定ページから確認できます。
merge_request_iid = 1  # 対象のマージリクエストのIIDを指定します。マージリクエストのURL末尾にある数値に相当します。

[api]
provider = openai  # 使用するAPIプロバイダーを指定します。openai または azure を指定できます。
openai_api_key = your_openai_api_key  # OpenAIのAPIキーを指定します。OpenAIのアカウントから取得できます。
chatgpt_model_name = gpt-4 # 使用するOpenAIのモデル名を指定します。通常はgpt-4を指定します。
azure_openai_api_key = your_azure_openai_api_key  # Azure OpenAIのAPIキーを指定します。Azureポータルから取得できます。
azure_openai_api_version = 2024-02-15-preview  # Azure OpenAIのAPIバージョンを指定します。通常はデフォルトのままで問題ありません。
azure_openai_endpoint = https://your-azure-endpoint  # Azure OpenAIのエンドポイントを指定します。Azureポータルから確認できます。

[locale]
language = Japanese  # コードレビューのフィードバックを提供する言語を指定します。例: Japanese, English

[ssl]
verify = True  # requests.get()でSSLの証明書の検証をするかどうか。基本的には必ずTrueにすること。
```

### メイン処理の実行

1. **config.iniの設定**:
    - 上記の内容を参考に、`config.ini`ファイルを適切に設定してください。

2. **スクリプトの実行**:
    - 以下のコマンドを実行して、マージリクエストのコードレビューを開始します。

    ```sh
    python main.py
    ```

3. **出力の確認**:
    - スクリプトが正常に実行されると、マージリクエストのコミット情報とコードレビューのフィードバックがコンソールに表示されます。

## 注意事項

- **APIキーの管理**:
  - APIキーは機密情報です。第三者に漏洩しないように注意してください。
  - `config.ini`ファイルは、外部に公開しないようにしてください。

- **GitLabのアクセス権限**:
  - プライベートトークンには、対象プロジェクトへのアクセス権限が必要です。適切な権限を持つトークンを使用してください。

- **APIの利用制限**:
  - OpenAIおよびAzure OpenAIのAPIには利用制限があります。詳細は各プロバイダーのドキュメントを参照してください。
