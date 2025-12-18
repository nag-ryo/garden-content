```ts
const func = (
	{ test, arg, context }
) => {
	console.log(test, arg, context); // 'test arg con'
}

func({
	test: 'test',
	arg: 'arg',
	context: 'con'
});
```

以下のように書く必要はない。
```ts
const func = (
	args: { test, arg, context }
) => {
	console.log(args.test, args.arg, args.context); // 'test arg con'
}

func({
	test: 'test',
	arg: 'arg',
	context: 'con'
});
```