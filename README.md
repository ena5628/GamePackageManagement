# Game Package Management

## 概要

ゲームパッケージを管理するWebアプリケーションです。

ゲームタイトル、ゲームの種類、プレイ状況、レビューなどを登録・管理できます。

## 開発動機
<p>Udemyでフロントエンド、バックエンドの基礎、および Docker による環境構築を学んだ際の学習アウトプットとして作成しました。</p>
<p>学んだ知識を統合し、実際に動作するアプリケーションを構築することで、Web開発の全体像を実践的に理解することを目的としています。</p>

## 前提条件
このプロジェクトを実行するには、以下のツールがインストールされている必要があります。
### **Docker / Docker Compose**

## 開発環境
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">

## 使用技術

### フロントエンド
<img src="https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white"> <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white"> <img src="https://img.shields.io/badge/CSS-663399?style=for-the-badge&logo=css&logoColor=white"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black">



### バックエンド
<img src="https://img.shields.io/badge/Node.js-5FA04E?style=for-the-badge&logo=nodedotjs&logoColor=white"> <img src="https://img.shields.io/badge/Express-0A0A0A?style=for-the-badge&logo=express&logoColor=white">

### データベース
<img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white">

#### その他ライブラリ
- nodemon: Node.jsアプリケーションを自動的に再起動してくれるツール（開発効率を上げるため）
- multer : アップロードされたファイルをImageディレクトリに保存するために使用（ミドルウェア）
- cors   : 異なるオリジン（ドメイン・ポート等）間での通信を安全に許可するために使用
- dotenv : .envファイル情報をNode.jsで扱えるようにするために使用
- mysql2 : Node.jsからMySQLデータベースを操作する際に使用

## 機能

- ゲーム一覧表示
- ゲーム情報登録
- ゲーム情報編集
- ゲーム情報削除
- 項目フィルター検索（ゲームの種類別）
- 画像アップロード（backendフォルダ内のImageフォルダに画像データを格納）

## セットアップ手順

### 1. リポジトリを取得

```bash
git clone https://github.com/ena5628/PackageGameManagement.git　
```

### 2. .envファイルの作成

```bash
cd ./PackageGameManagement
cp .env.example .env
```
>.env.example はあくまで設定項目のテンプレートです。
本番環境や個人環境で使用する際は、必ずより強固で推測されにくいパスワード等の値に書き換えてから使用することを強く推奨します。

### 3. docker composeの起動

```bash
docker compose up -d --build
```
## 工夫した点

開発環境と本番環境の差異による動作不具合を防ぐため、Dockerを導入しました。

docker-composeを用いてフロントエンド・バックエンド・データベースを複数コンテナとして構成し、Dockerfileによって環境構築をコードとして管理しています。

これにより、環境依存の問題を排除し、開発環境から本番環境へのスムーズな移行を実現しました。

## 苦労した点

本番環境としてAWSのEC2にデプロイして動かしましたが、フロントエンドで使用したfetchのURLがローカル環境用のままであり本番環境でうまく動作しない問題が生じました。

対策としてリバースプロキシを使いfetchURLをIPアドレス指定でなくドメイン形式に変更し、フロントとバックエンドの通信をコンテナ間通信に変更することで解決しました。

また画像データの取り扱いについても課題がありました。
ユーザーが保存した画像をバックエンドのフォルダに保存し、フロントエンドに表示されるうえでの画像保存であったりパス管理の設計に苦労しました。

最終的には、取得した画像データをBlobとして扱い、一時的なBlob URLを生成してフロントエンドに表示する方法と、もう一つは、サーバー上に画像を保存し、静的ファイルとしてアクセス可能なURLを発行して表示する方法を使い、一連の処理を実現することができました。

## 今後の課題・改善点

課題として、ユーザーごとの認証機能がなくデータも共有している状態なのでユーザーごとに切り分けてデータを表示・管理する必要があります。

また、デザインや機能面で改善できる点があるのでそこも修正していきたいです。
今回はEC2インスタンスにDockerソフトウェアをインストールして自分で管理をする必要があり、運用上の負担が課題となっています。

最終的にはAmazon ECS + Fargetを使用し、運用上の負担を軽減していく必要があると考えています。

DBについてもRDSに移行、画像データもサーバー上のEBSボリューム内に保存するのではなくS3に保存・取り出しする処理に変更する必要があります。
