# clab-devcontainer

このリポジトリは、OpenConfig を簡単に学習・実験するための環境を提供します。
DevContainer を利用することで、Nokia SR Linux 上で OpenConfig を試すための環境を迅速に構築できます。

## 特徴

- **DevContainer**: VS Code と Docker さえあれば、複雑なセットアップなしですぐに利用を開始できます。Docker-in-Docker (DinD)構成を採用しているため、ホスト環境を汚さずにネットワークシミュレーションが可能です。
- **Containerlab**: `clab` コマンドを使用してネットワークトポロジーを簡単にデプロイできます。

## 前提条件

- Docker
- Visual Studio Code
- [Dev Containers 拡張機能 (VS Code)](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## クイックスタート

1. **リポジトリを開く**:
   このリポジトリを VS Code で開きます。

2. **コンテナで開く**:
   VS Code の左下に表示されるプロンプト、またはコマンドパレット (`Cmd+Shift+P` / `Ctrl+Shift+P`) から **"Dev Containers: Reopen in Container"** を選択します。
   初回起動時はコンテナのビルドとセットアップに数分かかる場合があります。

3. **トポロジーのデプロイ**:
   コンテナが起動したら、統合ターミナルで以下のコマンドを実行してサンプルラボを展開します。

   ```bash
   clab deploy -t example.clab.yml
   ```

   > **Note**: コンテナ内で `clab` コマンドを実行する際、適切な権限とパス設定を行うためのエイリアスが自動的に設定されています。

## トポロジー構成

デフォルトの `example.clab.yml` は以下の構成を展開します。

- **Node**: 2台の SR Linux (`srl1`, `srl2`)
- **Link**: `srl1:e1-1` <--> `srl2:e1-1`
