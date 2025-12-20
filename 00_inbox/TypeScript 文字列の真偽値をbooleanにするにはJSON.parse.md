`Boolean();`を使いたくなるが、これはtruty・falsyに変換するものなので、`'true'`と`'false'`は両方ともtrueになってしまう。

正解はJSON.parse()を使用すること。
ただしundefinedはエラーになるので注意。

```ts
const test = 'true';
console.log(JSON.parse(test));
// true

const und = undefined;
console.log(JSON.parse(und));
// Unexpected token u in JSON at position 0

const nul = null;
console.log(JSON.parse(nul));
// null
```