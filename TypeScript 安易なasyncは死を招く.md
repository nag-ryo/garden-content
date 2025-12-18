filterメソッドのコールバック関数でasyncを付けたが、これが災いした。

```ts
const oopArr = ['Java', 'TypeScript', 'Python', 'Ruby'];
const myLangs = ['COBOL', 'FORTRUN', 'C', 'Java'];

const matchedLangs = myLangs.filter(async (l) =>{
	console.log(oopArr.includes(l)); // false,false,false,true
	return oopArr.includes(l);
});

console.log(matchedLangs); // ['COBOL', 'FORTRUN', 'C', 'Java'] Javaのみの想定が、全て入っている
```

async関数ではPromiseを返すので、filterメソッドが解決される前に処理が先に進んでしまう。
filterメソッドではasyncは使うべきではないし、そもそも使わないのであればasyncは付けてはならない。

```ts
const oopArr = ['Java', 'TypeScript', 'Python', 'Ruby'];
const myLangs = ['COBOL', 'FORTRUN', 'C', 'Java'];

const matchedLangs = myLangs.filter((l) =>{
	console.log(oopArr.includes(l)); // false,false,false,true
	return oopArr.includes(l);
});

console.log(matchedLangs); // ['Java']
```