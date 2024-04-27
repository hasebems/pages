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

- UARTをMIDIにする設定
    - [5pinDIN MIDIボードの販売ページ](https://booth.pm/ja/items/4441709)
    - [Raspberry Pi の UART 速度の調整](https://elchika.com/article/7a8ff868-5cb7-4fec-9976-a2fc69677d73/#h_Raspberry%20Pi%20%E3%81%AE%20UART%20%E9%80%9F%E5%BA%A6%E3%81%AE%E8%AA%BF%E6%95%B4)
    - [Raspberry Pi GPIOを使用したシリアル通信](https://www.ingenious.jp/articles/howto/raspberry-pi-howto/gpio-uart/)
    - [GPIO で UART を使う](https://hassiweb.gitlab.io/memo/docs/memo/raspberry-pi/raspberry-pi-os/mini-uart/)
    - [Need to update UART documentation for Pi5
](https://github.com/raspberrypi/documentation/issues/3239)
        - For example, on Pi4 with bullseye, specifying `console=serial0,115200` in cmdline.txt when `uart_enable=1` and/or `dtoverlay=disable-bt` is asserted in config.txt results in serial0 being linked as primary (i.e. ttyAMA0) and the console being enabled on pins 14/15 on J8 enabling firmware boot and kernel messages on boot and the system console via the J8 pins. In terms of aliases we have `/dev/serial0 -> ttyAMA0` and `/dev/serial1 -> ttyS0`. All good.  
        Not so on Pi5.  
        Instead on Pi5 with bookworm this configuration seems to enable all this on new UART connector. A different setup is needed to replicate backward compatible behaviour which setup seems to be: specify `console=ttyAMA0,115200` in cmdline.txt when `dtparam=uart0=on` is asserted in config.txt. All good, but confusing as now `/dev/serial0 -> ttyAMA10` where (I'm guessing ttyAMA10 is the new port) and /dev/serial1 is not assigned at all? If we also assert `dtoverlay=disable-bt` in config.txt we get aliases `/dev/serial0 -> ttyAMA10` and `/dev/serial1 -> ttyS0`.
        - serial0 is meant to be an alias for the "main" UART. On all Pis before Pi 5 this is obviously the UART enabled on GPIOs 14 & 15 (pins 8 & 10). This is the UART that a serial console would be enabled on (if wanted) by the `console=serial0,115200` in cmdline.txt. Pi 5 changes this by making the UART on the 3-pin debug header the default for serial0 - /dev/ttyAMA10. You can override this behaviour with `dtparam=uart0_console`, which enables UART0 and makes serial0 an alias for it.
        - serial1 is an alias for the UART used for Bluetooth, which is not accessible on the 40-pin header. However, on Bookworm (more accurately, since the newer 6.1 kernels) the kernel configures the UART for Bluetooth in a way that it has no `/dev/tty...` entry. As a result there is no `/dev/serial1. N.B`. It is still possible to revert to the old behaviour using `dtparam=krnbt=off`, but this isn't guaranteed to work forever.
    - [掲示板でRpi5×UART×MIDIの話](https://forums.raspberrypi.com/viewtopic.php?t=365126)
    - [公式のマニュアル](https://www.raspberrypi.com/documentation/computers/configuration.html#configuring-uarts)
    - で、結局どうなったかというと、
        - sudo raspi-config で、Interface Options -> Serial Port -> No -> Yes
        - /boot/firmware/config.txt に以下を書く
            enable_uart=1  
            init_uart_baud=38400  
            dtoverlay=midi-uart0-pi5  
            dtoverlay=disable-bt  
            dtparam=uart0=on  
        - この状態で、serial0, serial1 両方とも使えず、プログラムからは `/dev/ttyAMA0` でアクセスする
