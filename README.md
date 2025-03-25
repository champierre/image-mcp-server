# image-mcp-server

画像のURLを受け取り、GPT-4-turboモデルを使用して画像の内容を分析するMCPサーバーです。

## 機能

- 画像URLを入力として受け取り、その画像の内容を詳細に分析
- GPT-4-turboモデルを使用した高精度な画像認識と説明
- 画像URLの有効性チェック機能

## インストール

```bash
# リポジトリをクローン
git clone https://github.com/champierre/image-mcp-server.git
cd image-mcp-server

# 依存パッケージのインストール
npm install

# TypeScriptのコンパイル
npm run build
```

## 設定

このサーバーを使用するには、OpenAI APIキーが必要です。以下の環境変数を設定してください：

```
OPENAI_API_KEY=your_openai_api_key
```

## MCPサーバーの設定

Clineなどのツールで使用するには、MCPサーバー設定ファイルに以下の設定を追加してください：

### VSCode Claude拡張機能の場合

`cline_mcp_settings.json`に以下を追加：

```json
{
  "mcpServers": {
    "image-analysis": {
      "command": "node",
      "args": ["/path/to/image-mcp-server/dist/index.js"],
      "env": {
        "OPENAI_API_KEY": "your_openai_api_key"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### Claude Desktop Appの場合

`claude_desktop_config.json`に以下を追加：

```json
{
  "mcpServers": {
    "image-analysis": {
      "command": "node",
      "args": ["/path/to/image-mcp-server/dist/index.js"],
      "env": {
        "OPENAI_API_KEY": "your_openai_api_key"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## 使用方法

MCPサーバーが設定されると、以下のツールが利用可能になります：

- `analyze_image`: 画像URLを受け取り、その内容を分析します

### 使用例

```
画像URLを分析してください: https://example.com/image.jpg
```

### 画像処理機能とタイムアウト設定

このサーバーには以下の画像処理機能とタイムアウト設定が組み込まれています：

- 画像の自動縮小処理:
  - 最大サイズ: 512x512ピクセル
  - JPEG形式に変換（品質60%）
  - 元のサイズを維持（拡大はしない）

- OpenAIクライアント: 120秒（グローバル設定）
- 画像URLの検証:
  - 通常のドメイン: 60秒
  - gyazo.comドメイン: 120秒（特別設定）
- 画像サイズ制限: 10MB
- リトライ機能:
  - 最大3回のリトライ
  - 2秒の待機時間

大きな画像や応答の遅いサーバーからの画像を処理する場合でも、画像の自動縮小処理とリトライ機能によって成功率が向上します。それでもタイムアウトエラーが発生した場合は、以下の対処方法を試してください：

1. 別の画像URLを試す
2. 画像サイズが小さい画像を使用する（10MB未満）
3. 後でもう一度試す

注意: 
- 画像の取得にはGETリクエストを使用しているため、大きな画像の場合はダウンロードに時間がかかる場合があります。
- 一部の画像ホスティングサービス（特にgyazo.com）は、アクセス制限を設けている場合があります。このサーバーでは、ブラウザのユーザーエージェントを模倣してアクセス制限を回避する試みを行っています。
- 画像は一時ファイルとして保存され、分析後に自動的に削除されます。

## 開発

```bash
# 開発モードで実行
npm run dev
```

## ライセンス

ISC
