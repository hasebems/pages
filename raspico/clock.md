# Digital Clockを作る(2025/8)

[前に戻る](rp-pico.md)

ラズパイのRTCを動かす
-------------------------

- RTCを動かすには以下の2つのライブラリが必要
    - RP2040_RTC
    - TimeLib
- RP2040_RTC
    - Library Manager でRP2040_RTCを選び、インストール（なぜか最初エラーが出たが、ボードやライブラリを最新にしたらインストール出来た）
- TimeLib
    - [https://github.com/PaulStoffregen/Time]
    - 上記サイトのマニュアルにAPIが記載されているので、必要に応じて参照のこと
    - .zipを落として、「スケッチ」->「ライブラリをインクルード」->「.zip形式のライブラリをインストール」でファイルを指定
- スケッチ例から RP2040_RTC_Time をロードし、#include <TimeLib> を追加して、コンパイル

ラズパイで7segLEDを光らせる
--------------------------

- TM1637 のライブラリを使用
    - [https://github.com/avishorp/TM1637]
    - Library Manager で TM1637 を選び、インストール

サンプルプログラム作成
-----------------------



