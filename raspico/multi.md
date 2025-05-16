# ArduinoでMultiCore通信

[前に戻る](rp-pico.md)

FIFO 通信の方法
----------------

- 以下にサンプルコードを示す。 
- ★★★ が FIFO 通信に関わるコード
- 下記のように、rp2040.fifo.xxx() というAPIを利用する
    - rp2040 とあるが、RP2350 でも動作する

```
#include "pico/multicore.h"
#include "Arduino.h"

void setup() {
    // GPIO25をLED制御用に設定
    pinMode(25, OUTPUT);
}

void loop() {
    // FIFOからCore1のメッセージを受信してLED状態を変更
    if (rp2040.fifo.available()>0) {  // ★★★
        uint32_t data = rp2040.fifo.pop(); // ★★★
        if (data == 0x80) {digitalWrite(25, HIGH);}
        else {digitalWrite(25, LOW);}
    }
}

bool state = false;
void setup1() {
}

void loop1() {
    // Core1側: 200msecごとにon/off状態をFIFOで送信
    uint32_t data = 0x80;
    if (state) {data = 0xc0;}
    rp2040.fifo.push(data);  // ★★★
    state = !state;         // 状態反転
    delay(200);
}
```

共有メモリによる通信方法
----------------------------

- 下記にメモリ共有でリングバッファを作ったコードを示す

```
// Core間通信用
volatile uint32_t midiRcvRingBuffer[BUFFER_SIZE];
volatile int midiRcvWritePtr = 0;
volatile int midiRcvReadPtr = 0;

// 送る側
  int nextHead = (midiRcvWritePtr + 1) % BUFFER_SIZE;
  if (nextHead == midiRcvReadPtr) return false;  // バッファ満杯
  midiRcvRingBuffer[midiRcvWritePtr] = data;
  midiRcvWritePtr = nextHead;

// 受ける側
  if (midiRcvWritePtr == midiRcvReadPtr) return false;  // バッファ空
  uint32_t data = midiRcvRingBuffer[midiRcvReadPtr];
  midiRcvReadPtr = (midiRcvReadPtr + 1) % BUFFER_SIZE;


```


