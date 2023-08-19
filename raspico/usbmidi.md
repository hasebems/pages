# USB MIDI開発資料

[前に戻る](rp-pico.md)

そもそものUSBの資料
--------------------

- USB MIDI一般資料
    [https://www.usb.org/sites/default/files/midi10.pdf](https://www.usb.org/sites/default/files/midi10.pdf)
- USBデバイスクラスのID
    [https://www.usb.org/defined-class-codes](https://www.usb.org/defined-class-codes)
    10h がオーディオクラス, Sub ClassはMIDI: 03h
- TinyUSB Github : C++で書かれた USBスタック
    [https://github.com/hathach/tinyusb](https://github.com/hathach/tinyusb)


USB Crate/USB MIDI Sample
----------------------------

- [usb-device: https://docs.rs/usb-device/latest/usb_device/](https://docs.rs/usb-device/latest/usb_device/)
    - この Crate が必要っぽい。cargo.toml に追加する(usb-device = "0.2.9")。
- [udbd-midi: https://docs.rs/usbd-midi/0.2.0/usbd_midi/](https://docs.rs/usbd-midi/0.2.0/usbd_midi/)
    - 普通に MIDI 用の Crate が存在した。これも cargo.toml に追加。
    - 以下はこれを使ったサンプル
    - [https://github.com/Windfisch/analog-synth/tree/master/firmware-rust](https://github.com/Windfisch/analog-synth/tree/master/firmware-rust)
    - [https://github.com/btrepp/rust-midi-stomp](https://github.com/btrepp/rust-midi-stomp)
    - [https://github.com/pedalboard/pedalboard-midi](https://github.com/pedalboard/pedalboard-midi) : Raspberry Pi pico
    - [https://github.com/nmoutschen/rp-keypad](https://github.com/nmoutschen/rp-keypad) : Raspberry Pi pico
