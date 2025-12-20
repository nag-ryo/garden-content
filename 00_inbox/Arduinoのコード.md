言語はCPP
つまりC++

Arduinoでは、メモリ管理やポインタが不要になるように、以下のような魔法の関数を用意している。

## setup()
電源ON直後に1回だけ呼ばれる。

```cpp
const int LED_PIN = 8;
void setup() {
	pinMode(LED_PIN, OUTPUT);
}
```

## pinMode()
これはとても重要。
上記のsetup()でのコードで言えば、
「8番ピンは、電気を出す口ですよ」
とArduinoに教えている。

### 現実の意味
- OUTPUT = 蛇口
- INPUT = センサー

## loop()
世界を回す場所。

```cpp
void loop() {
	...
}
```

永遠に繰り返される空間。
Arduinoは思っている、「ここに書いてあること、死ぬまでやるね」

## digitalWrite()

```cpp
digitalWrite(LED_PIN, HIGH);
```

上記は何をしているのか？
- 8番ピンを HIGH = 5V にする
- 電気を出す

回路的には：
- 電圧がかかる
- 電流が流れる

簡単に言えば、
HIGH = ON、LOW = OFF
でよい。

## delay()

```cpp
delay(1000);
```

これは「待つ」命令。
- 単位はミリ秒

