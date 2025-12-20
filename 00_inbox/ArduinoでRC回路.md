
## 作るもの
PWM（カクカク）→ RC（なだらか）を作って、Arduinoで測る。

- PWM出力：D9（おすすめ。PWMピンならだいたいOK）
- RC回路：RとC
- 計測：A0（RCの途中の電圧を読む）

## 用意するパーツ
- 抵抗：10kΩ
	- 4.7kΩ〜100kΩ でもOK
- コンデンサ：0.1μF(104)か 1μF


## 配線
### RC低域フィルタ（ローパス）
- D9 → R → 出力点 → C → GND
- 出力点からは A0へ

```
D9 --- R ---+--- A0
            |
            C
            |
           GND
```

コンデンサが電解（プラマイがあるもの）なら、プラスを出力点、マイナスをGNDにする。

## コード
PWMのduty（明るさ）をゆっくり変えて、A0で電圧の変化を読む。

```cpp
const int PWM_PIN = 9;   // PWM出力
const int READ_PIN = A0; // RCの出力点を読む

void setup() {
  Serial.begin(115200);
  pinMode(PWM_PIN, OUTPUT);
}

void loop() {
  // 0→255→0へゆっくり変化
  for (int duty = 0; duty <= 255; duty++) {
    analogWrite(PWM_PIN, duty);
    int v = analogRead(READ_PIN);  // 0〜1023
    Serial.print(duty);
    Serial.print(",");
    Serial.println(v);
    delay(10);
  }
  for (int duty = 255; duty >= 0; duty--) {
    analogWrite(PWM_PIN, duty);
    int v = analogRead(READ_PIN);
    Serial.print(duty);
    Serial.print(",");
    Serial.println(v);
    delay(10);
  }
}
```

ツール→シリアルプロッタ を開く。
RCが効いていると、A0の値がなめらかに追従する。

## RCが効いていると何が見える？

RCなし（直結でA0を読む）と、
A0はガタガタすることが多い（PWMのカクカクが残る）

RCありだと、A0がぬるっと動く。
これが丸まったってこと。

## 物理の意味（数式→現象→音）

時定数
`τ = R × C`

例：10kΩ x 0.1μF
= 10000 x 0.0000001
= 0.001秒(1ms)

- τが大きいほど：反応が遅い（ヌルい、丸い）
- τが小さいほど：反応が速い（キビキビ、角が残る）

### 音で言うと

RCローパスは、ざっくりこう：
- 高音（ギラつき、エッジ、歯擦音っぽい成分）を削る
- 低音（太さ、丸さ）を残す

ギターで言えば、トーンを絞る方向。