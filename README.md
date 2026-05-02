# secure-dev-bootstrap

開発環境・セキュリティ設定・CI/CD・Issue/PR 作成フローを標準化するためのテンプレートリポジトリです。

このリポジトリを起点に、新規プロジェクトの初期設定を安全かつ再実行可能な形で整備します。

---

## 目的

このリポジトリの目的は、開発開始前に必要な設定を統一し、プロジェクトごとの差分や属人化を減らすことです。

主に以下を標準化します。

* 開発環境の初期設定
* 使用ツールのバージョン管理
* エディタ設定
* Git hooks
* セキュリティチェック
* CI/CD
* Renovate による依存関係更新
* Issue / Pull Request の作成フロー
* リポジトリ運用ルール

---

## 想定する利用方法

このリポジトリは、通常のアプリケーションコードを置くためのリポジトリではなく、各プロジェクトの雛形として利用することを想定しています。

利用方法は以下のどちらかです。

1. GitHub の Template Repository として利用する
2. このリポジトリを clone して、必要な設定を対象プロジェクトへ取り込む

---

## 初期セットアップ

リポジトリを clone した後、以下を実行します。

```bash
git clone git@github.com:OWNER/secure-dev-bootstrap.git
cd secure-dev-bootstrap
./bin/bootstrap
```

`bootstrap` は、開発に必要なツールや Git hooks をセットアップするための入口です。

---

## よく使うコマンド

### 初期セットアップ

```bash
./bin/bootstrap
```

### 開発環境の確認

```bash
./bin/doctor
```

### セキュリティチェック

```bash
./bin/security-check
```

### Issue 作成

```bash
./bin/issue "Issue title"
```

### Pull Request 作成

```bash
./bin/pr
```

base branch を指定する場合は以下のように実行します。

```bash
./bin/pr develop
```

---

## ディレクトリ構成

```text
.
├── bin/
│   ├── bootstrap
│   ├── doctor
│   ├── security-check
│   ├── issue
│   └── pr
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
├── .github/
│   ├── renovate.json
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── security.yml
│   │   ├── create-issue.yml
│   │   └── create-pr.yml
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
├── .vscode/
├── docs/
├── .editorconfig
├── .pre-commit-config.yaml
├── mise.toml
├── SECURITY.md
├── CONTRIBUTING.md
└── README.md
```

---

## セットアップ方針

このリポジトリのセットアップ処理は、以下の方針で作成します。

* 何度実行しても壊れない
* ホスト OS への破壊的な変更を行わない
* 必要なコマンドが不足している場合は明示的にエラーにする
* ローカルと CI で同じコマンドを使う
* セキュリティ関連の設定を後付けにしない

---

## 必要なツール

利用前に、以下のツールが必要です。

```text
- git
- docker
- mise
- pre-commit
- GitHub CLI
```

各ツールの導入方法は `docs/setup.md` に記載します。

---

## Renovate

依存関係の更新は Renovate で管理します。

設定ファイルは以下に配置します。

```text
.github/renovate.json
```

Renovate では、主に以下を管理します。

* GitHub Actions
* Dockerfile
* Docker Compose
* npm dependencies
* Python dependencies
* Go modules
* Terraform providers

プロジェクトに不要な manager は、各リポジトリ側で削除または無効化してください。

---

## CI/CD

CI/CD は GitHub Actions を前提にします。

基本方針は以下です。

* Pull Request 作成時に CI を実行する
* main branch への push 時に CI を実行する
* セキュリティチェックは定期実行する
* ローカルと CI で同じスクリプトを使う

主な workflow は以下です。

```text
.github/workflows/ci.yml
.github/workflows/security.yml
.github/workflows/create-issue.yml
.github/workflows/create-pr.yml
```

---

## Issue 作成

Issue は GitHub の Issue Template を利用して作成します。

テンプレートは以下に配置します。

```text
.github/ISSUE_TEMPLATE/
```

ローカルから Issue を作成する場合は、以下を実行します。

```bash
./bin/issue "Issue title"
```

---

## Pull Request 作成

Pull Request は共通テンプレートを利用します。

テンプレートは以下に配置します。

```text
.github/pull_request_template.md
```

ローカルから Pull Request を作成する場合は、以下を実行します。

```bash
./bin/pr
```

PR は draft として作成し、内容確認後に ready for review に変更する運用を基本とします。

---

## セキュリティ

セキュリティに関する方針は `SECURITY.md` に記載します。

このリポジトリでは、最低限以下を扱います。

* secret の混入防止
* 依存関係更新
* Git hooks
* branch protection
* Pull Request review
* CI 上の security check
* セキュリティ報告フロー

セキュリティ上の問題を見つけた場合は、公開 Issue ではなく `SECURITY.md` の手順に従って報告してください。

---

## 開発ルール

開発ルールは `CONTRIBUTING.md` に記載します。

基本ルールは以下です。

* main branch へ直接 push しない
* 作業は feature branch で行う
* Pull Request を作成してレビューを受ける
* CI が成功してから merge する
* セキュリティチェックを通過してから merge する

---

## Template Repository として使う場合

このリポジトリを GitHub の Template Repository として設定し、新規プロジェクト作成時にこのテンプレートを選択します。

新規リポジトリ作成後は、以下を実行してください。

```bash
./bin/bootstrap
./bin/doctor
./bin/security-check
```

プロジェクト固有の内容に合わせて、以下を編集します。

```text
README.md
SECURITY.md
CONTRIBUTING.md
.github/renovate.json
.github/workflows/*.yml
mise.toml
```

---

## 注意事項

このリポジトリは、各開発者の PC に対して無条件に設定変更を行うものではありません。

ホスト OS への変更は最小限にし、可能な限り以下で管理します。

* Dev Container
* mise
* pre-commit
* GitHub Actions
* Renovate

開発者の環境に影響する処理を追加する場合は、必ず事前に README または docs に明記してください。

---

## 今後追加するもの

今後、以下を追加予定です。

* `bin/bootstrap`
* `bin/doctor`
* `bin/security-check`
* `bin/issue`
* `bin/pr`
* `.github/renovate.json`
* `.github/workflows/ci.yml`
* `.github/workflows/security.yml`
* `.github/ISSUE_TEMPLATE/*`
* `.github/pull_request_template.md`
* `SECURITY.md`
* `CONTRIBUTING.md`
* `docs/setup.md`

---

## License

このリポジトリのライセンスは、プロジェクト
