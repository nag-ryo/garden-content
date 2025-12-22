実験日：2025/12/21

## やりたいこと
- ボタンを押すと7セグのカウントが上がる

## やってみる

### 配線
7セグを光らせたときのものをベースにする。

ボタンをブレッドボードに配置して、それをA0などで検知するのだと思う。
→違った。
### 内部プルアップ

配線は
```
ボタン片側→GND
ボタン反対側→D9
```

コード側
```cpp
pinMode(9, INPUT_PULLUP);
```

これで何が起きるのか？
- 押していない
	- ピンはHIGH
- 押す
	- GNDに繋がってLOW

押したらLOWなのは直感と真逆なので注意

## 作ってみた

配線。7セグ回路の右に[[ボタンの回路]]を追加した。
![[ボタン回路と7segの写真.jpeg]]


コード
```cpp
int segPins[7] = {2,3,4,5,6,7,8};

void setup() {
  pinMode(9, INPUT_PULLUP);
  for (int i=0; i<7; i++) {
    pinMode(segPins[i], OUTPUT);
    digitalWrite(segPins[i], LOW); // まず全部消す
  }
}

int count=0;

void loop() {
  if (digitalRead(9) == LOW) {
    count++;

    if (count > 9) {
      count = 0;
    }
  }

  switch (count) {
    case 0:
      show0();
      break;
    case 1:
      show1();
      break;
    case 2:
      show2();
      break;
    case 3:
      show3();
      break;
    case 4:
      show4();
      break;
    case 5:
      show5();
      break;
    case 6:
      show6();
      break;
    case 7:
      show7();
      break;
    case 8:
      show8();
      break;
    case 9:
      show9();
      break;
    default:
      reset();
      break;
  }
}


//   a
// f   b
//   g
// e   c
//   d

// ピン配列
int a=4, b=5, c=8, d=7, e=6, f=3, g=2;

void reset() {
  digitalWrite(a,LOW);
  digitalWrite(b,LOW);
  digitalWrite(c,LOW);
  digitalWrite(d,LOW);
  digitalWrite(e,LOW);
  digitalWrite(f,LOW);
  digitalWrite(g,LOW);
}

void show0() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,HIGH);
  digitalWrite(f,HIGH);
  digitalWrite(g,LOW);
}

void show1() {
  digitalWrite(a,LOW);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,LOW);
  digitalWrite(e,LOW);
  digitalWrite(f,LOW);
  digitalWrite(g,LOW);
}

void show2() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,LOW);
  digitalWrite(d,HIGH);
  digitalWrite(e,HIGH);
  digitalWrite(f,LOW);
  digitalWrite(g,HIGH);
}

void show3() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,LOW);
  digitalWrite(f,LOW);
  digitalWrite(g,HIGH);
}

void show4() {
  digitalWrite(a,LOW);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,LOW);
  digitalWrite(e,LOW);
  digitalWrite(f,HIGH);
  digitalWrite(g,HIGH);
}

void show5() {
  digitalWrite(a,HIGH);
  digitalWrite(b,LOW);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,LOW);
  digitalWrite(f,HIGH);
  digitalWrite(g,HIGH);
}

void show6() {
  digitalWrite(a,HIGH);
  digitalWrite(b,LOW);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,HIGH);
  digitalWrite(f,HIGH);
  digitalWrite(g,HIGH);
}

void show7() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,LOW);
  digitalWrite(e,LOW);
  digitalWrite(f,LOW);
  digitalWrite(g,LOW);
}

void show8() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,HIGH);
  digitalWrite(f,HIGH);
  digitalWrite(g,HIGH);
}

void show9() {
  digitalWrite(a,HIGH);
  digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
  digitalWrite(d,HIGH);
  digitalWrite(e,LOW);
  digitalWrite(f,HIGH);
  digitalWrite(g,HIGH);
}
```


ただ、このままだとボタンを押している間、ずっとカウントアップする。

ボタンが切り替わった瞬間だけcountアップしたい。
ボタンを押している間がLOWなので、HIGHから切り替わった瞬間だけ、countアップに流れるようにする。

前回のStateがHIGHかつ、現在のStateがLOWならカウントアップ。

コードを修正した。ついでにチャタリング対策。
```cpp
lastButtonState == HIGH;
void loop() {
  int currentState = digitalRead(buttonPin);
  if (lastButtonState == HIGH && currentState == LOW) {
    delay(50); // チャタリング待ち
    if (digitalRead(buttonPin) == LOW) { // 待ったあとでも押されているなら本物とする
      count++;
      if (count > 9) {
        count = 0;
      }
    }
  }
  lastButtonState = currentState;  
  // ...他のコード
}
```

## ボタンで学んだこと
- INPUT_PULLUPはLOWが押下
- 押した瞬間はHIGH→LOW
- ボタンは震える（チャタリング）
- delay(50)は確定の儀式


[[7セグの表示関数を共通化|次回]]へ続く...