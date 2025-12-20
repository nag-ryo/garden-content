## Array.forEach()
Promise.all()を使用することもできるが、for...ofでawaitを使えばいいので、これが1番手っ取り早いと思われる。

## Array.map()
1. mapが呼び出す関数をasyncにする。
2. mapの中で呼び出す非同期関数はawaitする。
3. mapがreturnする配列をPromise.allに格納する。
4. Promise.allをawaitで同期させる。

```ts
// ダミー処理
function funcPromise(b){
    return new Promise((resolve, reject)=>{
        if(b){
            resolve("OK");
        } else {
            reject("ERROR");
        }
    });
}

(async ()=>{

    const array = [1,2,3];

    const result = await Promise.all(array.map(async (v)=>{
        const dummy = await funcPromise(true);
        return dummy;
    }));

    console.log(result);    // ["OK", "OK", "OK"]
})();

```

参考文献: [JavaScriptの配列のmapでasync/awaitを使う方法 - Qiita](https://qiita.com/kwbt/items/6c0fe424c89a9f7553c1)

## Array.filter()

例えば以下のロジックは思った通りの動作をしない
```ts
(async () => {
	const arr = [1,2,3,4,5];
	const filArr = await arr.filter(async (num) => {
		await sleep(1000);
		if (num % 2 === 0) {
			console.log('偶数です');
			return true;
		} else {
			console.log('奇数です');
			return false;
		}
	});
	console.log(filArr);
})()
async function sleep(milliseconds: number) {
	return new Promise<void>((resolve) => setTimeout(resolve, milliseconds));
}
// [1,2,3,4,5]
// 奇数です
// 偶数です
// 奇数です
// 偶数です
// 奇数です
```

asyncFilter()を作成する。
通常のfilterと同じ感覚で使用できる。
```ts
(async () => {
	const arr = [1,2,3,4,5];
	const filArr = await asyncFilter(arr, async (num: number) => {
		await sleep(2000);
		if (num % 2 === 0) {
			console.log('偶数です');
			return true;
		} else {
			console.log('奇数です');
			return false;
		}
	});
	console.log(filArr);
})()

async function sleep(milliseconds: number) {
	return new Promise<void>((resolve) => setTimeout(resolve, milliseconds));
}

async function asyncFilter(array: unknown[], asyncCallback) {
	const bits = await Promise.all(array.map(asyncCallback));
	return array.filter((_, i) => bits[i]);
}
// 奇数です
// 偶数です
// 奇数です
// 偶数です
// 奇数です
// [2,4]
```

[[TypeScriptでfilterの頭にawaitを置いても同期はしないので注意]]
## 参考文献
[JavaScriptで配列のfilterにasync関数を使いたい- Qiita](https://qiita.com/hnw/items/f104a1079906fc5c2a96)
