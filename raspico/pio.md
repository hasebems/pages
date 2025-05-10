# ArduinoでPicoのPIO機能を使う

---

今回は、以下の内容を説明します：

1. Arduino-Picoコアは自動的にpioasmを呼び出さないので、ヘッダーファイルを自分で生成する必要があります。
2. 方法：

   * CLIツールを使用する場合、コアにバンドルされたpioasmバイナリを探します（例：「/Library/Arduino15/packages/rp2040/tools/pioasm/1.1.2/pioasm」）。Windowsの場合、パスは「%LOCALAPPDATA%\Arduino15\packages\rp2040\tools\pioasm...」です。また、Wokwiのオンラインツールを使うこともできます。
3. コマンド例：

```
   pioasm -o c-sdk blink.pio blink.pio.h
```


---

スケッチのコードは以下のようになります：

```cpp
#include "hardware/pio.h"
#include "blink.pio.h"

void setup() {
  PIO pio = pio0;
  uint offset = pio_add_program(pio, &blink_program); // SM0を選択
  uint sm = 0;
  blink_program_init(pio, sm, offset, 25); // LED接続ピン
}

void loop() {}
```

7. `blink_program_init` 関数がヘッダーでどのように定義されているかも紹介します。

8. その他のヒント：

   * `pioasm`のコンパイル方法（ホスト内でまたはオンライン）。
   * 頻繁に編集する場合、PlatformIOの`extra_script`やArduino CLIの`pre-build`スクリプトで自動的に`pioasm`を実行できます。
   * 手動で行う場合、`static const uint16_t` メソッドを使用することもできます。Adafruit RP2040 Arduinoコア（Earleではなく）を使用する場合、SDKが含まれていないことがあるので、コア開発者のパスを確認する必要があります。


---

Arduino RP2040用のpico coreは自動的にpioasmを実行しないため、ヘッダーの生成は手動で行う必要があります。

ツールとしては：

* オプション1: Arduino coreに含まれているpioasmバイナリ。コマンド: `pioasm -o c-sdk myprog.pio myprog.pio.h`
* オプション2: Wokwiオンラインのpioasm
* オプション3: プレビルドスクリプトやカスタムビルド

手順：

1. `.pio`ファイルの作成
2. pioasmを使ってヘッダーを生成
3. ヘッダーをスケッチフォルダーに配置
4. Arduinoスケッチを書く
5. `hardware/pio.h`とヘッダーをインクルード
6. ステートマシンを初期化

このようにして、メンテナンスや注意点にも触れつつ進めます。


思考時間: 1m 30s


### Arduino環境で `.pio` を使う基本フロー

1. **PIOアセンブリを書く**
   まず SDK そのままの書式で `myprog.pio` を作成します。

2. **`pioasm` で C++ ヘッダに変換する**
   Arduino-Pico コア（Earle Philhower 版）には *バイナリ版の pioasm* が同梱されています。

   ```bash
   # macOS/Linux の例
   $HOME/Library/Arduino15/packages/rp2040/tools/pioasm/**<ver>**/pioasm \
       -o c-sdk myprog.pio myprog.pio.h

   # Windows の例
   "%LOCALAPPDATA%\\Arduino15\\packages\\rp2040\\tools\\pioasm\\<ver>\\pioasm.exe" ^
       -o c-sdk myprog.pio myprog.pio.h
   ```

   `-o c-sdk` を指定すると Pico-SDK 互換の `myprog.pio.h` が生成され、
   ─ `const pio_program myprog_program`
   ─ `static inline void myprog_program_init(...)`
   など一式が自動生成されます。([arduino-pico.readthedocs.io][1])

   > **手元にツールが無い場合**
   >
   > * オンライン版 [https://wokwi.com/tools/pioasm](https://wokwi.com/tools/pioasm) でも同じヘッダを生成できます。([wokwi.com][2])
   > * CLI のインストールが面倒な場合はここで編集中の `.pio` を貼り付けて download するだけです。

3. **スケッチにヘッダを置き `#include`**
   `myprog.pio.h` をスケッチフォルダにコピーし、Arduino 側では

   ```cpp
   #include "hardware/pio.h"
   #include "myprog.pio.h"   // ②で生成したヘッダ

   void setup() {
     PIO pio      = pio0;
     uint sm      = 0;                 // 使用するステートマシン
     uint offset  = pio_add_program(pio, &myprog_program);

     // pioasm が用意した初期化関数を呼ぶ
     myprog_program_init(pio, sm, offset, /*GPIO*/ 0 /*ほか必要な引数*/);
     pio_sm_set_enabled(pio, sm, true);
   }

   void loop() {}
   ```

   Pico-SDK 関数 (`pio_add_program`, `pio_sm_*`) は Arduino-Pico コアにフル実装済みです。([GitHub][3])

---

### 運用のヒント

| 課題                          | 代表的な解決策                                                                                           |
| --------------------------- | ------------------------------------------------------------------------------------------------- |
| **編集のたびに pioasm を手で叩くのが面倒** | Arduino-CLI なら `prebuild` スクリプト、PlatformIO なら `extra_script.py` で pioasm を自動実行できます。               |
| **PIO プログラムを 32 語以内に収めたい**  | pioasm は 33 語以上になるとエラー表示。分割ロードや２台目 PIO ブロックで回避できます。                                               |
| **ヘッダを使わずに埋め込みたい**          | `static const uint16_t prog[] = {0xe081, …};` を手書きすればビルドは通りますが、ラベル解決・config 生成を自前で行う必要があり保守が大変です。 |
| **Adafruit RP2040 コアを使う**   | SDK が最小構成なので `#include "hardware/pio.h"` が見つからない場合があります。Earle 版か wizio-pico へ切り替えると楽です。           |

---

### まとめ

* **Arduino ではビルド時に自動で `.pio → .h` へ変換されない**ため、
  *事前に* `pioasm -o c-sdk`（もしくは Wokwi Online）でヘッダを用意しておく。
* 生成ヘッダをスケッチに `#include` すれば Pico-SDK と同じ API で PIO を扱える。
* 量産する場合はビルドフックで自動生成するワークフローを整えると快適。

これで Arduino 環境でも公式 SDK と同じ手順で PIO を活用できます。

[1]: https://arduino-pico.readthedocs.io/en/latest/sdk.html "Using the Raspberry Pi Pico SDK (PICO-SDK) — Arduino-Pico 4.5.3 documentation"
[2]: https://wokwi.com/tools/pioasm?utm_source=chatgpt.com "pioasm Online - Wokwi"
[3]: https://github.com/earlephilhower/arduino-pico/discussions/221?utm_source=chatgpt.com "How to use custom Pio files ? · earlephilhower arduino-pico - GitHub"
