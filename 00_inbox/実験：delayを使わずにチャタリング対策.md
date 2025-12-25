実験日：2025/12/24

## 動機

「[[実験：ボタンでLEDを操る]]」では、delay()を使ったチャタリング対策を行った。
しかしこれには問題がある。
delayはArduinoに「何もするな」と命令していることになる。
- ボタンを押しても反応無し
- 別の処理があっても無視
- 世界が止まる
という問題を抱えている。
例えるなら、1つの仕事しか出来ない人になる。

そこで今回は、delayではない方法でチャタリング対策を行うことに挑戦する。

## やってみよう

使うのはこの関数
```cpp
millis();
```

これは、電源が入ってからの経過時間（ミリ秒）を出力する。
これを見て、delay()と同じ挙動を再現する。

### 今回の設計

1. ボタンの状態を見る
2. 押された瞬間ならcountを増やす

これだけ。

### コードを書く

最終的に以下のようになった。
```cpp
// 左から、a用のピン、b用のピン、のように並んでいる
int segPins[7] = {4,5,8,7,6,3,2};
int buttonPin = 9;

void setup() {
  pinMode(9, INPUT_PULLUP);
  for (int i=0; i<7; i++) {
    pinMode(segPins[i], OUTPUT);
    digitalWrite(segPins[i], LOW); // まず全部消す
  }
}

int count=0;
// ボタン用
int stableState = HIGH;
int lastReading = HIGH;
unsigned long lastChangeTime = 0;
const unsigned long debounceMs = 30;

void loop() {
  // ボタンが押されたらLOW
  int reading = digitalRead(buttonPin);

  // 変化を検知したら時刻を記録
  if (reading != lastReading) {
    lastChangeTime = millis();
  }

  // 一定時間変わってなければ本物とする
  if ((millis() - lastChangeTime) > debounceMs) {
    if (reading != stableState) {
      stableState = reading;

      // 押された瞬間
      if (stableState == LOW) {
        count++;
        if (count > 9) count = 0;
      }
    }
  }

  lastReading = reading;

  showDigit(count);
}

// 数字*10 と セグメント*7
const byte digits[10][7] = {
  // a b c d e f g
  {1,1,1,1,1,1,0}, // 0
  {0,1,1,0,0,0,0}, // 1
  {1,1,0,1,1,0,1}, // 2
  {1,1,1,1,0,0,1}, // 3
  {0,1,1,0,0,1,1}, // 4
  {1,0,1,1,0,1,1}, // 5
  {1,0,1,1,1,1,1}, // 6
  {1,1,1,0,0,0,0}, // 7
  {1,1,1,1,1,1,1}, // 8
  {1,1,1,1,0,1,1}  // 9
};

void showDigit(int n) {
	for (int i = 0; i < 7; i++) {
		digitalWrite(segPins[i], digits[n][i] ? HIGH : LOW);
	}
}
```

loop()内の挙動を翻訳すると、
1. 値が変わったら「今の時刻」をメモ
2. 30ms 以上落ち着いたら「本気の入力」と認める
3. LOWになった瞬間だけ count++
4. 表示は今のcountを出す

これで正常な動作を確認した。
## 実験は成功だ！

これでArduinoを止めることがなく、delay()を使わずにチャタリング対策を行うことが出来る。
時間を「待つ」ものから「条件」に変えたのだ。
delay()で世界を止めるな！