# Rust開発環境構築 & 開発方法

[前に戻る](rp-pico.md)


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
上記を利用して、開発フォルダを作りたい場所で以下をタイプ（バックスラッシュの後 return して改行）
    > cargo generate \
       --git https://github.com/rp-rs/rp2040-project-template \
       --branch main \
       --name xxxxxxxx

    で、環境が一発で作成できる（名前に _ を使うと怒られて、 - に変えられる）

1. Cargo.toml を修正
name をプロジェクトの名前に

1. .cargo/config.toml のコメント修正(cargo.tomlじゃないよ！)
    - 実行バイナリを転送するrunner修正
    > #runner = "probe-run --chip RP2040"
    > runner = "elf2uf2-rs -d"
    > #runner = "probe-rs run --chip RP2040 --protocol swd"


テンプレートへの機能追加の方法
----------------------------

- [https://github.com/rp-rs/rp-hal/tree/main/rp2040-hal/examples](https://github.com/rp-rs/rp-hal/tree/main/rp2040-hal/examples)
    これらのプログラムを参考にしてペリフェラル(I2C, UART, SPIなど)を使用するプログラムを作成する。


書き込み方法
-------------------

- ラズパイpicoのスイッチを押しながら、USBケーブルをPCのUSBに挿す
- RPI-RP2 というドライブがPCに現れる
- cargo run で書き込みが始まる
- 正常に書き込み終了すると、「ディスクの不正な取り出し」が現れ、PCから RPI-RP2 のドライブが消える
- ドライブが消えない場合
    - cargo run をもう一度行ってみる
    - ときどき、書き込み中にエラーが起こり、ドライブが見えるようになる。なぜかプログラムも書かれている。
    - 今のところ、ドライブが消えない場合のリカバリ方は上記のやり方でしかうまくいっていない
