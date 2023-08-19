# Rust開発環境構築 & 開発方法

Raspberry Pi pico向けPage
------------------------------

- https://docs.rs/crate/rp-pico/latest  : raspi pico 向け crate
- https://github.com/rp-rs/rp-hal-boards/tree/main/boards/rp-pico : 上記の github
- Crateの包含関係の考え方
    - rp2040-hal : RP2040マイコンの基本的機能
    - rp-pico : マイコンにボード(Raspberry Pi pico)の情報を載せたもの？


picoの環境構築
--------------

https://qiita.com/ochaochaocha3/items/1969d76debd6d3b42269
上記記事より

- >rustup target install thumbv6m-none-eabi
    - Cortex-M0のtoolchainをインストール
- >cargo install flip-link elf2uf2-rs
    - flip-linkは、組込みプログラム実行時のスタックオーバーフローを防止するため、リンク時にメモリ配置を変更するツール
    - elf2uf2-rsは、ビルドしたプログラムをUF2というPicoに書き込める形式に変換するツール


Templeteから環境構築
----------------------

以下の手順で環境作成
1. pico開発用Templeteがあるので、そこから新規作成  https://github.com/rp-rs/rp2040-project-template
上記を利用して
> cargo generate \
    --git https://github.com/rp-rs/rp2040-project-template \
    --branch main \
    --name xxxxxxxx

で、環境が一発で作成できる（名前に _ を使うと怒られて、 - に変えられる）

2. Cargo.toml を修正
name をプロジェクトの名前に

1. .cargo/config.toml のコメント修正(cargo.tomlじゃないよ！)
    - 実行バイナリを転送するrunner修正
>runner = "probe-run --chip RP2040"
>runner = "elf2uf2-rs -d"
>runner = "probe-rs run --chip RP2040 --protocol swd"


テンプレートへの機能追加の方法
----------------------------

https://github.com/rp-rs/rp-hal/tree/main/rp2040-hal/examples
これらのプログラムを参考にしてペリフェラル(I2C, UART, SPIなど)を使用するプログラムを作成する。
