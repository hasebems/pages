# ラズパイで Loopian_Rust を動かそうとした話

[前に戻る](raspi_main.md)

1. Rust のインストール
    - 'curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh'

1. Loopian_Rust のリポジトリ作成
    - `https://github.com/hasebems/Loopian_Rust.git`

1. Loopian_Rust のビルド(24/04/12-)
    - 最初に以下のエラーが出る
        - `error: Failed to run custom build command for 'alsa-sys v0.3.1'`
    - 以下のパッケージをロードする
        - `sudo apt install libasound2-dev libudev-dev pkg-config`
    - 次に、ファイルが見つからないとエラーが出たので、フルパス指定に変更
    - MIDI Port 設定
        - MIDI_OUT : "Midi Through:Midi Through Port-0 14:0"