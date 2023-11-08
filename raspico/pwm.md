# PWM

[前に戻る](rp-pico.md)

PWM出力の参考ページ

- [Raspberry Pi PicoでPWM出力](https://lipoyang.hatenablog.com/entry/2021/12/12/201432) : 個人ブログ
- [Raspberry Pi PicoとMOSFET ③ PWMのAPI](https://www.denshi.club/parts/2021/07/raspberry-pi-pico-18-pwm.html) : 個人ブログ
- PWMの関数
    - *analogWriteFreq(uint32_t frequency)*
    : to set the frequency of the PWM signal. It supports a frequency of 100Hz to 1MHz*.
    - *analogWriteRange(uint32_t range)*
    : to set the range of the PWM signal — the maximum value you’ll use to set 100% duty cycle. It supports a value from 16 to 65535.
    - *analogWriteResolution (int resolution)*
    : to set the resolution of the PWM signal — up to 16-bit.
    - *analogWrite(GPIO, duty cycle)*
    : to output a PWM signal to a specified pin with a defined duty cycle. It continuously outputs a PWM signal until a digitalWrite() or other digital output is performed.