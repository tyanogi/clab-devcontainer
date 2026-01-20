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

1. **リポジトリのクローン**:
   サブモジュールを含めてリポジトリをクローンします。
   ```bash
   git clone --recursive https://github.com/tyanogi/clab-devcontainer.git
   ```
   > **Note**: 既にクローン済みの場合は、`git submodule update --init --recursive` を実行してください。

2. **リポジトリを開く**:
   クローンしたディレクトリを VS Code で開きます。

3. **コンテナで開く**:
   VS Code の左下に表示されるプロンプト、またはコマンドパレット (`Cmd+Shift+P` / `Ctrl+Shift+P`) から **"Dev Containers: Reopen in Container"** を選択します。
   初回起動時はコンテナのビルドとセットアップに数分かかる場合があります。

4. **トポロジーのデプロイ**:
   コンテナが起動したら、統合ターミナルで以下のコマンドを実行してサンプルラボを展開します。

   ```bash
   clab deploy -t example.clab.yml
   ```

   > **Note**: コンテナ内で `clab` コマンドを実行する際、適切な権限とパス設定を行うためのエイリアスが自動的に設定されています。

## トポロジー構成

デフォルトの `example.clab.yml` は以下の構成を展開します。

- **Node**: 2台の SR Linux (`srl1`, `srl2`)
- **Link**: `srl1:e1-1` <--> `srl2:e1-1`

## 動作確認

ラボが正常に展開された後、`gnmic` を使用して OpenConfig 経由で情報を取得できるか確認します。

1. **ターゲットIPの取得**:
   ```bash
   srl1=$(clab inspect -w -f json 2> /dev/null | jq -r '.srl01[] | select(.name | endswith("srl1")) | .ipv4_address | split("/")[0]')
   ```

2. **OpenConfig でホスト名を取得**:
   ```bash
   gnmic -a $srl1:57401 get --insecure -u admin -p NokiaSrl1! --encoding JSON_IETF --path /openconfig/system/state/hostname
   ```

   **実行結果例**:
   ```json
   [
     {
       "source": "172.20.20.2:57401",
       "timestamp": 1768117867726518870,
       "time": "2026-01-11T07:51:07.72651887Z",
       "updates": [
         {
           "Path": "openconfig-system:system/state/hostname",
           "values": {
             "openconfig-system:system/state/hostname": "srl1"
           }
         }
       ]
     }
   ]
   ```
