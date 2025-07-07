# Express.js + Docker 学習プロジェクト

## 🎯 学習目的

環境構築をサクッと行うための基本技術（JavaScript/Docker）の習得

## 📋 プロジェクト構成

```
Docker_Assignment1/
├── Dockerfile          # Docker環境の設定
├── package.json         # Node.jsの依存関係とスクリプト
├── app.js              # Express.jsアプリケーション本体
└── README.md           # このファイル
```

## 🔧 使用技術

- **Node.js**: JavaScript 実行環境
- **Express.js**: Web アプリケーションフレームワーク
- **Docker**: コンテナ化技術

## 📚 学習のポイント

### 1. Express.js の基本構造

Express.js は最小限のコードで Web サーバーを構築できる Node.js フレームワークです。

#### app.js の詳細解説

```javascript
const express = require("express"); // Express.jsモジュールの読み込み
const app = express(); // Expressアプリケーションインスタンスの作成
const port = 3000; // サーバーのポート番号を定義
```

**ルーティングの仕組み**

- `app.get("/", (req, res) => {})`: HTTP の GET リクエストを処理
- 第 1 引数 `"/"`: URL パス（この場合はルートパス）
- 第 2 引数: コールバック関数（リクエストとレスポンスを処理）
- `res.send()`: クライアントにレスポンスを送信

**サーバー起動**

- `app.listen(port, callback)`: 指定されたポートでサーバーを起動
- コールバック関数でサーバー起動完了時の処理を定義

### 2. Dockerfile の基本構成と最適化

Dockerfile は、コンテナイメージを作成するための設計図です。

#### 各命令の詳細解説

**ベースイメージの選択**

```dockerfile
FROM node:lts-slim
```

- `node:lts-slim`: Node.js LTS 版の軽量イメージ
- LTS（Long Term Support）により安定性を確保
- slim タグで不要なパッケージを排除し、イメージサイズを縮小

**作業ディレクトリの設定**

```dockerfile
WORKDIR /app
```

- コンテナ内での作業場所を`/app`に設定
- 以降の命令はこのディレクトリで実行される

**効率的なファイルコピー戦略**

```dockerfile
COPY package*.json ./
RUN npm install
COPY . .
```

- **重要**: package.json を先にコピーする理由
  - Docker 層キャッシュの活用
  - package.json が変更されない限り、npm install をスキップ
  - ビルド時間の大幅短縮

**依存関係のインストール**

```dockerfile
RUN npm install
```

- package.json に記載された依存関係をインストール
- この時点で node_modules ディレクトリが作成される

**ポートの公開**

```dockerfile
EXPOSE 3000
```

- コンテナが使用するポートを宣言
- 実際のポートマッピングは`docker run`時に指定

**起動コマンドの指定**

```dockerfile
CMD ["npm", "start"]
```

- コンテナ起動時に実行されるデフォルトコマンド
- package.json の scripts セクションの"start"を実行

### 3. package.json の役割と構成

Node.js プロジェクトの設定ファイルで、プロジェクトの全体像を定義します。

#### 各フィールドの詳細

**基本情報**

```json
{
  "name": "express-docker-app", // プロジェクト名
  "version": "1.0.0", // バージョン番号
  "description": "A simple Express app for Docker", // 説明
  "main": "app.js" // エントリーポイント
}
```

**スクリプト定義**

```json
{
  "scripts": {
    "start": "node app.js" // npm startコマンドの定義
  }
}
```

- `npm start`実行時に`node app.js`が起動
- 他にも`npm run [script名]`で任意のスクリプトを実行可能

**依存関係管理**

```json
{
  "dependencies": {
    "express": "^4.19.2" // プロダクション依存関係
  }
}
```

- `^4.19.2`: セマンティックバージョニング（4.x.x の最新版を使用）
- `npm install`時にこの情報を元にパッケージがインストールされる

### 4. Docker 層キャッシュの最適化

Dockerfile の記述順序は、ビルドパフォーマンスに大きく影響します。

#### 最適化の原理

1. **変更頻度の低いものを上位に配置**

   - ベースイメージ（FROM）
   - システムパッケージのインストール
   - 依存関係（package.json）

2. **変更頻度の高いものを下位に配置**
   - アプリケーションコード
   - 設定ファイル

#### 具体的な効果

- 初回ビルド: 全層を構築（時間がかかる）
- 2 回目以降: 変更されていない層はキャッシュを使用（高速）
- コード変更時: COPY . .以降のみ再実行

## 🚀 実行手順

### Docker 環境での実行

```bash
# Dockerイメージをビルド
docker build -t express-docker-app .

# コンテナを起動
docker run -p 3000:3000 express-docker-app
```

### ローカル環境での実行

```bash
# 依存関係をインストール
npm install

# アプリケーションを起動
npm start
```

アプリケーションは http://localhost:3000 でアクセス可能です。

## 🎯 学習成果

- Node.js + Express.js の基本的な Web アプリケーション構築
- Docker を使った環境の標準化とポータビリティの実現
- Docker 層キャッシュを活用した効率的なビルド手法
- コンテナ化による環境依存の問題解決

## 📝 メモ

このプロジェクトは環境構築の基礎技術習得を目的としており、実際の開発では追加のセキュリティ対策や最適化が必要です。
