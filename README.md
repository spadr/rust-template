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
