#### TypeScript 里的工具类型 Partial

- Partial<T> 可以快速把某个接口类型中定义的属性变成可选的
  ```
  interface People {
  age: number;
  name: string;
  }

	const Jerry:People = {
		age: 10,
		name: 'Jerry'
	};

	const Tom: People = {
		name: 'Tom'
	}
	// 报错 age是必输
	```

	```
	type AnonymousPeople = Partial<People>;

	// 源码
	type Partial<T> = {
    [P in keyof T]?: T[P];
	};
	```

