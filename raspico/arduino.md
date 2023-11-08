# Arduino開発環境

[前に戻る](rp-pico.md)

- Arduino環境のすすめ
    - 公式が提供しているC++開発環境は、Linux環境を前提としていることもあり、ハードルが高く、その分維持コストも高い
    - ボード価格の安い Raspberry pi pico をWSなどで使いたいことも多く、その場合、少ない時間ですぐに環境構築できるArduinoは魅力的

- 実は公式から出ているArduino環境のドキュメント
    - [https://arduino-pico.readthedocs.io/en/latest/index.html](https://arduino-pico.readthedocs.io/en/latest/index.html)

- 参考記事
    - [Raspberry Pi Pico をArduinoでプログラミング！](https://karakuri-musha.com/inside-technology/arduino-raspberrypi-pico-helloworld01/) : Web記事

- Arduino環境下でのAPI
    - PWMの関数
        - *analogWriteFreq(uint32_t frequency)*
        : to set the frequency of the PWM signal. It supports a frequency of 100Hz to 1MHz*.
        - *analogWriteRange(uint32_t range)*
        : to set the range of the PWM signal — the maximum value you’ll use to set 100% duty cycle. It supports a value from 16 to 65535.
        - *analogWriteResolution (int resolution)*
        : to set the resolution of the PWM signal — up to 16-bit.
        - *analogWrite(GPIO, duty cycle)*
        : to output a PWM signal to a specified pin with a defined duty cycle. It continuously outputs a PWM signal until a digitalWrite() or other digital output is performed.