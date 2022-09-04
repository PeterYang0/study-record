#### TypeScript之装饰器

- 为了使Typescript编译器支持装饰器，需要在tsconfig.json的compilerOptions选项中设置"experimentalDecorators": true。
```
// tsconfig.json
{
  "compilerOptions": {
    ...
    "experimentalDecorators": true,
    ...
  }
}
```
