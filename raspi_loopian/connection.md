# Raspberry Pi で Loopian と Pianoteq8 を動かす

## RTP MIDI install

- Raspberry Pi へのインストールと動作チェック
    - [https://webmidiaudio.com/npage403.html](https://webmidiaudio.com/npage403.html)
- Mac 側の設定
    - [https://webmidiaudio.com/npage515.html#mactorpi](https://webmidiaudio.com/npage515.html#mactorpi)
    - 上のページにある通り、ディレクトリのプラスボタンを押して、RaspberryPiのIP addressを直接入力して、項目を追加する必要がある。
    - また、Mac側は、ライブルーティングに「ネットワーク○○」を選択しないと動かない。
- 上記記事のオリジナル
    - [https://blog.tarn-vedra.de/pimidi-box/](https://blog.tarn-vedra.de/pimidi-box/)
- ラズパイのファイアウォール設定
    - sudo iptables -L
    - sudo iptables -A INPUT -p tcp --dport 5900 -j ACCEPT

いろいろやったが、RTP MIDIはうまくいきそうになくて断念
* Macとラズパイを直接、有線LANケーブルで繋ぐのは一般的な接続方法ではなく、ネットワーク設定が面倒
    - 通常はハブやルーターから接続されている
* Macのインターネット共有をONにしたが、ラズパイがネットに繋がらなかった。

## Raspberry Pi 5

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
- Pianoteq
    - [掲示板でラズパイインストール話](https://forum.modartt.com/viewtopic.php?id=8268)
    - [ptq8&rpi5対応](https://github.com/Hillcow/pianoteq-pi/tree/main)
