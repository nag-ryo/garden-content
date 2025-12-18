テストなどでreadonlyの定数をモックしたいときがあるが、基本的にはできない。
## ソリューション
### 継承
定数をreadonlyにしていないBaseクラスを作成して、実装時はそれを継承してreadonlyにする。テストでは継承するがreadonlyにしないなどの方法が考えられる。

しかし、これだけのためにOOPの仕組みを使うのはイタズラに複雑化させるだけ。避けるべき。
### メソッド化
メソッドならモックできる。
まだこちらの方が現実的。
```ts
// クラス
private readonly _Constant = true;
private get Constant() {
	return this._Constant;
}
```

```ts
// テストファイル
spyOn(obj as any, 'Constant').and.returnValue(false);
```