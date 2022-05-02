#### TS 泛型＜ T ＞

- 泛型：在定义函数，接口，类的时候，不预先指定具体的类型，而在使用的时候在去指定类型的一种特征

  ```
  //函数泛型格式如下   函数方法名<T>(参数):返回值
  function createArray<T>(length: number, value: T): Array<T> {
      let result: T[] = [];
      for (let i = 0; i < length; i++) {
          result[i] = value;
      }
      return result;
  }
  createArray<string>(3, 'x'); // ['x', 'x', 'x']
  ```

- 泛型也可以理解为一种特殊的参数。例如函数的泛型的作用域就是整个函数作用域函数也支持使用多个泛型

  ```
  function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
    }

  swap([7, 'seven']); // ['seven', 7]
  ```

- 泛型约束
  在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性。这时候我们可以泛型 T 继承某个接口，那么传递过来的类必须要有这个属性才能传递过来
  ```
  interface Lengthwise {
		length: number;
	}

	function loggingIdentity<T extends Lengthwise>(arg: T): T {
		console.log(arg.length);
		return arg;
	}
	```

- 可以使用泛型接口的方式定义一个符合条件的函数
	```
	interface CreateArrayFunc {
		//<T>代表这个T在这个函数的作用域上面
		<T>(length: number, value: T): Array<T>;
	}

	//表示T的作用域在整个对象上面
	interface CreateArrayFunc<T> {
		(length: number, value: T): Array<T>;
	}
	```

- 泛型参数的默认类型
泛型的类型可以指定默认类型，当泛型没有在代码中指定参数类型，那么将启用默认参数类型
	```
	function createArray<T = string>(length: number, value: T): Array<T> {
		let result: T[] = [];
		for (let i = 0; i < length; i++) {
				result[i] = value;
		}
		return result;
	}
	```

- 接口函数
	```
	// 如果泛型添加在接口上面表示该泛型可以使用到整个接口
	interface AT<T> {
		(a:T,b:T):T
	}
	//这里就是确定泛型的类型 ，也可以吧a和b类型去掉，因为泛型已经确定类型
	let add :AT<number> = function<T>(a:number,b:number) :number{
		return a;
	}

	// 如果泛型定义在函数上面，那么泛型只能使用在一个函数上面.定义与使用如下所示
	interface AT {
         <T>(a:T,b:T):T
   }
    let add :AT = function<T>(a:T,b:T) :T{
      return a;
   }
	```