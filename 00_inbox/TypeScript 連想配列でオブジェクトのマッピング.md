```ts
const mappingArr: { [id: string]: number[]} = {};
arr.forEach((v, i) => {
	if (mappingArr[v.id]) {
		mappingArr[v.id].push(i);
	} else {
		mappingArr[v.id] = [i];
	}
});
```