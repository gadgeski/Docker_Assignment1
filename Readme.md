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

- `express()`でアプリケーションインスタンスを作成
- `app.get()`で GET リクエストのルーティングを定義
- `app.listen()`でサーバーを起動

### 2. Dockerfile の基本構成

- `FROM node:lts-slim`: 軽量な Node.js LTS イメージを使用
- `WORKDIR /app`: 作業ディレクトリを設定
- `COPY package*.json ./`: package.json を先にコピー（Docker 層キャッシュの最適化）
- `RUN npm install`: 依存関係をインストール
- `COPY . .`: アプリケーションコードをコピー
- `EXPOSE 3000`: ポート 3000 を公開
- `CMD ["npm", "start"]`: コンテナ起動時のコマンド

### 3. package.json の役割

- プロジェクトのメタデータ
- 依存関係の管理
- スクリプトの定義

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
