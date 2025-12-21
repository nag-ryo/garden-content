
[[Arduino]]に[[可変抵抗]]と抵抗とLEDを用意する。

## つなぎ方
![[20251220_Arduino_可変抵抗回路図.png]]

## Arduino側

```cpp
const int LED_PIN = 8;
const int POT_PIN = A0;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(9600); // 数字をのぞき見る
}

void loop() {
  int value = analogRead(POT_PIN); // 0〜1023
  Serial.println(value);

  digitalWrite(LED_PIN, HIGH);
  delay(value);        // つまみで待ち時間が変わる
  digitalWrite(LED_PIN, LOW);
  delay(value);
}
```

## シリアルをモニタリング
Serial.println()
の結果を見るには、
Tools/Serial Monitor


## 可変抵抗を0にすると

うっすら光る。
上記のコードでは、点ける→消すを超高速で繰り返すようになる。
超絶一瞬だが、光っている瞬間があり、人間の目はそれを平均して見るようになる。

## LEDの回路と可変抵抗の回路は全く別物

A0は電圧を見るだけ。電流を流す主役ではない。
[[Arduinoのピンの意味]]

回路図を書く場合、別々に書くことになる。

### 失敗例

![[可変抵抗回路図の失敗例.png|400]]
### 成功例（LED回路はシンプルなので省略）

学習用
```
      5V
       |
     /\/\/\   ← 可変抵抗
       |
      A0
       |
      GND
```

省略形
`5V - VR - A0 - GND`

玄人
`VR: 10k (5V-GND), wiper=A0`


玄人はこれを見た瞬間、
3本足のポテンショメーターで、真ん中がA0だなって判断できるとのこと。

