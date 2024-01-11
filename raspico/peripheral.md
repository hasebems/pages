# Rust Peripheral開発環境

[前に戻る](rp-pico.md)

用語解説(Terminology)
-----------------------

- BSP : Board Support Packages
- PAC : Peripheral Access Crates
- HAL : Hardware Abstruct Layer

Peripheral アクセス方法の整理
--------------------------------

- 全体イメージ
    - BSP, PAC, HAL の関係性
        - use 宣言の際、bsp::hal::pac の順にシンボルが繋がっている
        - HAL を具体的なチップの仕様に落としたのが、PAC
        - HAL の上に、ボードの仕様をかぶせたのが、BSP
        - そもそも HAL はハードの違いを吸収するためのものなので、ハードへのアクセスは PAC のシンボルを直接使うのではなく、できるだけ HAL の提供するAPIを使わなければならない
    - Cargo.toml には、BSP を書けば良い。(rp-pico)
        - BSP が rp2040-hal(PAC) を包含する。
        - 実際には、BSP(rp-pico)自体には、全ピンの定義しか入っておらず、別途 PAC を付属している。
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
    - BSP, PAC, HAL の関係性に関する記事: [https://qiita.com/iwatake2222/items/6bec963a145a15019ca5](https://qiita.com/iwatake2222/items/6bec963a145a15019ca5)


具体的な実装の問題点
------------------------------

- 現状の問題意識
    - main()関数内で宣言されたペリフェラルの変数を、main()関数以外に持ち運ぶことができない。
    - 引数に入れると、copy Traitがないのなんのと言われる

- 現状の対応
    1. ペリフェラルを変数に出来る型で、グローバル変数化する。
        - グローバル変数は "static mut 大文字" と宣言
        - 型は rust-analyzer などで調べる
    1. グローバル変数は、型の宣言を Option でくるみ、初期値を None にする。
    1. 初期化処理の中で、init関数などを呼んで、この変数を初期化する。
    1. unsafe などを頻繁に書かなくて済むように、このペリフェラルをアクセスする関数を別途作成しておく。
        - 関数内は unsafe 内に if let Some(x) = &mut ペリフェラル名 {} と書き、xでアクセス
    1. 上記の関数を任意の箇所からコールして使う

### ペリフェラル内のGenerics

```rust:i2c.rs
pub struct I2C<I2C, Pins, Mode = Controller> {
        i2c: I2C,
        pins: Pins,
        mode: PhantomData<Mode>,
}
```

- 上記の定義では、最初のI2Cが型であり、<>内にあるI2Cは、Genericsとして示す文字列
- 型指定にあるGenerics
    - 例えば: static mut USB_DEVICE: Option<UsbDevice<hal::usb::UsbBus>> = None;
        - Option は None があるから
        - UsbDevice<hal::usb::UsbBus> は ??
            - インスタンス生成なので、UsbDevice の型宣言の中にある型引数に、UsbBus が入れられたと考えられる
    - "A＜B＞" : AはGenericsを伴う型であり、中に特定の型Bを入れないと成り立たない。
        - BにさらにGenericsがあった場合、上記と同じ考えを繰り返す
        - 型定義でGenericsを使う場合、Trait で指示するが、Genericsで実インスタンスを作るときは、型名を指定しなければならない

