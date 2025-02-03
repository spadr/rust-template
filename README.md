# Rust Devcontainer Template

このリポジトリは、VSCodeのRemote Container (devcontainer) でRust開発を行うためのテンプレートです。

## 使い方

1. **Use this template** からリポジトリを生成、もしくはgit cloneで取得  
2. VSCodeでこのフォルダを開く  
3. 「Remote-Containers: Open Folder in Container」を実行  
4. devcontainerのビルド完了後、`cargo run` などで動作を確認  

### docker-composeのみで起動する場合

```bash
docker-compose up --build
```

コンテナ内で `cargo watch -x run` などを実行し、ソース変更時に自動ビルド＆実行できます。

---

## CIの使い方

本テンプレートには、以下のような GitHub Actions ワークフローが同梱されています。

1. **CI (ビルド & テスト)**  
   - `.github/workflows/ci.yml`  
   - プッシュやプルリクエスト時に `cargo build/test` および `cargo clippy` を実行

2. **Security audit**  
   - `.github/workflows/audit.yml`  
   - `cargo-audit` を使い、依存関係の脆弱性を定期スキャン

3. **Release**  
   - `.github/workflows/release.yml`  
   - タグ (`v*`) をプッシュすると、自動的にリリースを作成しバイナリをアップロード

### リポジトリをGitHub上で運用する場合

- メインブランチにプッシュするとCIが走り、ビルド・テストが実施されます  
- `v1.0.0` のようなバージョンタグをプッシュすると、リリースワークフローが走り、ビルド成果物をGitHubリリースに添付します  
- `audit.yml` はスケジュール (cron) で定期的に依存脆弱性をチェックします

---

## ローカルでCIをデバッグする (actの使い方)

開発時に、GitHub Actionsをローカルでテスト・デバッグしたい場合は **[act](https://github.com/nektos/act)** を使用します。

1. **actのインストール**  
   - WSLやLinux上で `brew install act`、あるいはバイナリを直接取得するなどしてセットアップ  
2. **トークンの設定**  
   - リリースワークフローやプライベートリポジトリ操作など、GitHubへの書き込みが必要な場合は、`GITHUB_TOKEN`としてPAT(Personal Access Token)を設定  
   - 例: `.github/workflows/act_secrets.env` に `GITHUB_TOKEN=ghp_xxxx` を書き、`act --secret-file .github/workflows/act_secrets.env` で実行
3. **実行コマンド例**  

```bash
# デフォルトで "push" イベントをシミュレート
act --secret-file .github/workflows/act_secrets.env

# pull_request イベントを試すなら
act pull_request --secret-file .github/workflows/act_secrets.env

# 特定のジョブだけ実行
act -j build_and_test
```

4. **ReleaseやSecurity auditをスキップする**  
   - リリースジョブは `if: env.ACT != 'true'` などの条件でローカルデバッグ時（`ACT=true`）にはスキップする設定にしています。  
   - セキュリティ監査（`audit.yml`）も同様に `act -j` で指定しなければ動かさないなど、必要に応じて調整可能です。

> **Note**: `act` 実行時にネットワーク状態やDockerイメージの相性でエラーが出る場合があります。何度か再実行すれば動くことも多いですが、頻発する場合は `act` のDockerイメージを変える、`cargo-audit` のDBをキャッシュする等の対策が有効です。

---

## その他Tips

- **Cargo.tomlの\[package\].nameを変更**した場合、ワークフロー内で`cargo metadata`を使って自動取得しています。ハードコードを避けるので、リポジトリ名とパッケージ名が異なっていても問題ありません。  
- **devcontainerの設定**(`.devcontainer/devcontainer.json`)でVSCode拡張やRustツールチェーンをインストールしているため、チーム全員が同じ環境を再現できます。  
- **VSCodeタスク(`.vscode/tasks.json`)**を定義している場合、`act` をワンコマンド実行するなどの作業を効率化できます。