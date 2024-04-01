# Arduino開発環境

[前に戻る](rp-pico.md)

- Arduino環境のすすめ
    - 公式が提供しているC++開発環境は、Linux環境を前提としていることもあり、ハードルが高く、その分維持コストも高い
    - ボード価格の安い Raspberry pi pico をWSなどで使いたいことも多く、その場合、少ない時間ですぐに環境構築できるArduinoは魅力的

- 実は公式から出ているArduino環境のドキュメント
    - [https://arduino-pico.readthedocs.io/en/latest/index.html](https://arduino-pico.readthedocs.io/en/latest/index.html)

- Arduinoの設定
    - 公式ではなく別のボードをインストールする。以下の URL を「追加のボードマネージャのURL」に貼り付ける
      https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json
    - Raspberry Pi Pico/RP2040 by Earle F.Philhower, III をインストール
    - ツールメニューから、USB Stack を、"Adafruit TinyUSB" に変更

- Arduino で MIDI Program を書く方法
    - MIDI Library のインストール
      https://github.com/FortySevenEffects/arduino_midi_library
    - TinyUSBのサンプルソース
      https://github.com/adafruit/Adafruit_TinyUSB_Arduino/blob/master/examples/MIDI/midi_test/midi_test.ino

- Arduino で Timer を使う
    - Arduino-Picoで動作する以下を使用する(今までのMsTimer2の代わり)
    - https://github.com/khoih-prog/RPI_PICO_TimerInterrupt
    - https://www.arduino.cc/reference/en/libraries/rpi_pico_timerinterrupt/




- LCD ACM1602NI-FLW-FBW-M01 の使い方
    - []

- 参考記事
    - [Raspberry Pi Pico をArduinoでプログラミング！](https://karakuri-musha.com/inside-technology/arduino-raspberrypi-pico-helloworld01/) : Web記事
