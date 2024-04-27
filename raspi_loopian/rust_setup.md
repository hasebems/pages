# ラズパイで Loopian_Rust を動かした話

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

1. Loopian_Rust で UART MIDI を受信させてみる(24/4/20-)
    - [Raspberry Pi でシリアルを使うお勉強](setup.md)
    - UART の初期化
        - 初期化は `Uart::with_path("/dev/ttyAMA0", 38400, Parity::None, 8, 1)`
        - 38400 bps に設定することに注意
    - [UART初期化時の設定](https://docs.rs/rppal/latest/rppal/uart/struct.Uart.html#method.set_read_mode)
        - 4つのmodeがあるが、スレッドを止めるわけにいかないので、Non-blocking read にする
        - `let _ = u.set_read_mode(0, Duration::ZERO);`