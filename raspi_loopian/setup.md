# Raspberry Pi 5の設定

[前に戻る](raspi_main.md)

- Getting Started
    - [公式サイト](https://www.raspberrypi.com/documentation/computers/getting-started.html)
    - [シリアル有効化](https://qiita.com/s_fujii/items/466d455ca19fb4c20744)
- Audio
    - [Board](https://docs.rs-online.com/1796/A700000006917300.pdf)
    - [Board公式サイト](http://www.inno-maker.com/hifi-dac-hat-for-raspberry-pi/)
    - [I2S設定](http://marchan.e5.valueserver.jp/cabin/comp/jbox/arc300/doc3008.html)
    - sudo nano /boot/firmware/config.txt
        - dtparam=i2s=on の # を取って enable に
        - dtoverlay=allo-boss-dac-pcm512x-audio を追記
- VNCの設定
    - [VNCの設定](https://www.indoorcorgielec.com/resources/raspberry-pi/raspberry-pi-vnc/)
    - [最新のRaspiOSでRealVNCが使えない問題の解決方法](https://qiita.com/konchi_konnection/items/c8e2258f0a7efb49302f)
