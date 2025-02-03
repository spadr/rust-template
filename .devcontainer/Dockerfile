# ベースイメージ
FROM rust:1.84-slim

# 基本的な開発ライブラリを導入
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    pkg-config \
    libssl-dev \
    ca-certificates \
    lldb \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# cargoツール類のインストール
RUN cargo install cargo-watch cargo-edit cargo-outdated cargo-release

# Rust標準コンポーネントの追加
RUN rustup component add clippy rustfmt

# 作業ディレクトリ
WORKDIR /workspace

# デフォルトコマンド
CMD ["bash"]