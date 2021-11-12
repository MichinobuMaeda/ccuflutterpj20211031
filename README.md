# 学習会 Firebase+Flutter 2021年10月31日

``PS>`` は PowerShell のプロンプトを表す。　Linux　や　Mac　はシェル上で同等のことができる。

``$`` は bash のプロンプトを表す。 Windows で WSL を利用しない場合は Git bash や PowerShell 上でも可。

## 開発環境

Windows 11

[Chrome](https://www.google.co.jp/chrome/)

以下、 Chocolatey を利用していますが、
[WSL: Windows Subsystem for Linux](https://docs.microsoft.com/ja-jp/windows/wsl/install)
をお勧めします。

[Chocolatey](https://chocolatey.org/install)

PowerShell を管理者として起動

```
PS> Get-ExecutionPolicy
Restricted

PS> Set-ExecutionPolicy AllSigned                                                                   
PS> Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
PS> choco --version
0.11.3

PS> choco install git -y
PS> exit
```

注意: Chocolatey をインストールするコマンドは必ず配布元の最新の情報を参照してください。

PowerShell を一般ユーザで起動 ( Git Bash でもよい )

```
PS> git --version
git version 2.33.1.windows.1

PS> git config --global user.name "Michinobu Maeda"
PS> git config --global user.email "michinobumaeda@gmail.com"
PS> exit
```

PowerShell を管理者として起動

```
PS> choco install vscode -y
PS> exit
```

PowerShell を一般ユーザで起動 ( Git Bash でもよい )

```
PS> code --version
1.61.2

PS> exit
```

PowerShell を管理者として起動

```
PS> choco install openjdk -y
PS> exit
```

PowerShell を一般ユーザで起動 ( Git Bash でもよい )

```
PS> java --version
openjdk 17.0.1 2021-10-19

PS> exit
```

PowerShell を管理者として起動

```
PS> choco install nodejs --version=14.18.1 -y
PS> exit
```

PowerShell を一般ユーザで起動 ( Git Bash でもよい )

```
PS> node--version
v14.18.1

PS> npm --version
6.14.8

PS> npm install yarn firebase-tools -g
PS> yarn --verison
yarn install v1.22.17

PS> firebase --version
9.21.0

PS> exit
```

PowerShell を管理者として起動

```
PS> choco install flutter -y
PS> exit
```

PowerShell を一般ユーザで起動 ( Git Bash でもよい )

```
PS> flutter --version
Flutter 2.5.3

PS> flutter


PS> flutter doctor

PS> exit
```

## Flutter のプロジェクトの作成

```
PS> flutter create --platforms=web ccuflutterpj20211031
PS> code ccuflutterpj20211031
```

VS Code でターミナルを開く

```
PS> flutter run -d chrome --web-port 5000
```

## GitHub 初期登録

GitHub
<https://github.com/MichinobuMaeda>

- Repositories
    - New
        - Repository name: ccuflutterpj20211031

```
PS> git init
PS> git add .
PS> git commit -m "first commit"
PS> git branch -M main
PS> git remote add origin git@github.com:MichinobuMaeda/ccuflutterpj20211031.git
PS> git push -u origin main
```

## プラットフォーム Android の追加

```
PS> flutter create --platforms=android .
PS> flutter run -d emulator-5554
```

## Git pull request

```
PS> git checkout -b changethemecolor
```

## Firebase のプロジェクトの作成

https://console.firebase.google.com/

- Add project
    - Project nane: ccuflutterpj20211031
    - Configure Google Analytics
        - Create a new account: ccuflutterpj20211031
        - Analytics location: Japan
- Project settings
    - Your apps
        - Select a platform to get started: </> (Web)
            - App nickname: Flutter test 20211031
    - Your project
        - Default GCP resource location: asia-northeast1 (Tokyo)
        - Public-facing name: ccuflutterpj20211031
        - Support email: my address
- Authentication
    - Sign-in providers
        - Email/Password: Enable
            - Email link (passwordless sign-in): Enable
    - Templates
        _ Template language: Japanese
- Firestore Database
    - Create Database: Start in production mode

```
$ firebase login

$ firebase init hosting
? What do you want to use as your public directory? build/web
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? No

$ firebase init hosting:github
? For which GitHub repository would you like to set up a GitHub workflow? (format: user/repository) MichinobuMaeda/ccuflutterpj20211031
? Set up the workflow to run a build script before every deploy? Yes
? What script should be run before every deploy? yarn install --frozen-lockfile && yarn build
? Set up automatic deployment to your site's live channel when a PR is merged? Yes
? What is the name of the GitHub branch associated with your site's live channel? main
i  Action required: Visit this URL to revoke authorization for the Firebase CLI GitHub OAuth App:
https://github.com/settings/connections/applications/89cf50f02ac6aaed3484
i  Action required: Push any new workflow file(s) to your repo
```

# サンプルアプリのデプロイ

GitHub Actions 上で firebase-tools を利用するために Node.js の設定を追加。　

```
$ npm init
$ yarn -D add firebase-tools
```

GitHub Actions のワークフロー

- .github
    - workflows
        - firebase-hosting-merge.yml
        - firebase-hosting-pull-request.yml

に、以下のタスクを追加する。

- Flutter
    - flutter コマンドのインストール
    - ``pubspec.yaml`` に指定されたパッケージの追加
    - ビルド ( Web )
- Node.js
    - node コマンド ( version: 14 ) のインストール
    - yarn コマンドのインストール
    - ``package.json`` に指定されたパッケージの追加

GitHub　に push する。　

```
$ git add .
$ git commit -m "修正内容等をここに書く。日本語可。"
$ git push
```

GitHub Actions の [ジョブ](https://github.com/MichinobuMaeda/ccuflutterpj20211031/runs/4186212125?check_suite_focus=true)
が自動で起動する。

<https://ccuflutterpj20211031.web.app/> にアプリがデプロイされる。


