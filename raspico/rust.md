# Rust開発環境構築 & 開発方法

[前に戻る](rp-pico.md)

Raspberry Pi pico向けPage
------------------------------

- [raspi pico 向け crate のRustドキュメント](https://docs.rs/crate/rp-pico/latest)
- Crateの包含関係の考え方
    - embedded-hal : [https://crates.io/crates/embedded-hal](https://crates.io/crates/embedded-hal)
        - Embedded Devices Working Group が開発している組み込み向け HAL (Traitを集めたCrate)
    - rp2040-hal : [https://docs.rs/rp2040-hal/latest/rp2040_hal/](https://docs.rs/rp2040-hal/latest/rp2040_hal/)
        - 上記 embedded-hal の RP2040マイコン向け実装
    - rp-pico : [https://github.com/rp-rs/rp-hal-boards/tree/main/boards/rp-pico : マイコンにボード(Raspberry Pi pico)の情報を載せたもの？](https://github.com/rp-rs/rp-hal-boards/tree/main/boards/rp-pico)
        - rp2040-hal を含んでいる
        - GPIO の定義が書かれた lib.rs がソースに追加されている
    - 上記とは全然別に、組み込み用USBのCrateが存在する (usb-device)
    - さらに上記の USB Crate とは別に、USB MIDI 用のCrateが存在する (usbd-midi)


picoの環境構築
--------------

[Qiita記事より](https://qiita.com/ochaochaocha3/items/1969d76debd6d3b42269)

- Cortex-M0のtoolchainをインストール
    >rustup target install thumbv6m-none-eabi
- 関連ツールのインストール
    >cargo install flip-link elf2uf2-rs
    - flip-linkは、組込みプログラム実行時のスタックオーバーフローを防止するため、リンク時にメモリ配置を変更するツール
    - elf2uf2-rsは、ビルドしたプログラムをUF2というPicoに書き込める形式に変換するツール


Templeteから環境構築
----------------------

以下の手順で環境作成する。
以下では、デバッグ用Probeは用いず、直接実行ファイルを書き込む方法を説明する。

1. pico開発用Templeteがあるので、そこから新規作成  [https://github.com/rp-rs/rp2040-project-template](https://github.com/rp-rs/rp2040-project-template)
上記を利用して
    > cargo generate \
    --git https://github.com/rp-rs/rp2040-project-template \
    --branch main \
    --name xxxxxxxx

    で、環境が一発で作成できる（名前に _ を使うと怒られて、 - に変えられる）

1. Cargo.toml を修正
name をプロジェクトの名前に

1. .cargo/config.toml のコメント修正(cargo.tomlじゃないよ！)
    - 実行バイナリを転送するrunner修正
    >runner = "probe-run --chip RP2040"
    >runner = "elf2uf2-rs -d"
    >runner = "probe-rs run --chip RP2040 --protocol swd"


テンプレートへの機能追加の方法
----------------------------

- [https://github.com/rp-rs/rp-hal/tree/main/rp2040-hal/examples](https://github.com/rp-rs/rp-hal/tree/main/rp2040-hal/examples)
    これらのプログラムを参考にしてペリフェラル(I2C, UART, SPIなど)を使用するプログラムを作成する。
