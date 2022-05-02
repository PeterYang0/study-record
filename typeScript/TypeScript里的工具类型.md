#### TypeScript 里的工具类型

```
export interface Contact{
  name: string; // 姓名
  phone?: string; // 手机号
  email: string; // 邮箱
  avatar: string; // 头像
  userid: string; // id
}
```

- Pick选取类型中指定类型
```
// 源码
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

export interface ContactPick extends Pick<Contact, 'name' | 'phone'> {}
ContactPick {
  name: string;
  phone?: string; 
}
```

- Omit去除类型中某些项
```
// 源码
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;

// 去除单个
export type OmitEmailContact = Omit<Contact, 'email' >;
OmitEmailContact {
  name: string;
  phone?: string; 
  avatar: string;
  userid: string;
}

// 去除多个
export type OmitEmailAvatarContact = Omit<Contact, 'email' | 'avatar'>;
OmitEmailContact {
  name: string;
  phone?: string; 
  userid: string;
}

```

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

- Required将类型中所有选项变为必选，去除所有?
```
// 源码
type Required<T> = {
    [P in keyof T]-?: T[P];
};

// 变为必选
export interface RequiredContact= Required<Contact>
RequiredContact{
  name: string; // 姓名
  phone: string; // 手机号
  email: string; // 邮箱
  avatar: string; // 头像
  userid: string; // id
}
```

- 自定义（withId）
```
// 实现
export type WithId<T, P = string> = T & { id: P };

// demo
export interface ContactWithId = WithId<Contact>;
```