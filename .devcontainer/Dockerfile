# ベースイメージはお好みでバージョン指定
FROM rust:1.84-slim

# 必要に応じてライブラリを追加
RUN apt-get update && apt-get install -y \
    lldb \
    gcc \
    git \
    && rm -rf /var/lib/apt/lists/*

# cargo-watchのインストール（ホットリロードに使用する場合）
RUN cargo install cargo-watch

# コンテナ内のワークディレクトリ作成
WORKDIR /workspace

# [オプション] Rustツールのコンポーネント (clippy, rustfmt) をインストール
RUN rustup component add clippy rustfmt

# 最後にデフォルトコマンド指定(ここでは何もしない)
CMD [ "bash" ]
