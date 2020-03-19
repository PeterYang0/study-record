## java基础学习
1. jdk,jre,jvm的慨念
  * JRE-java运行环境，包括java虚拟机(JVM)和核心类库。
  * JDK-提供给开发人员使用，包括了开发工具，也包括了JRE，所以安装了JDK不需要额外安装JRE

2. Hello world
```
public class demo01 {
    public static void main(String[] args) {
        System.out.printf("Hello world!");
    }
}
```

3. 关键字 

4. 常量
  - 字符串 "Hello world!"
  - 整数 12, -23 ,10
  - 字符常量 'a', 'b',
  - 小数 12.33
  - 布尔值 true, false
  - 空常量 null

5. 数据类型
  - 基本数据类型 4类8种
    - 整数       占用字节数不同
      - int       4个字节
      - long      8个字节
      - short     2个字节
      - byte      1个字节
    - 浮点
      - float     4个字节
      - double    8个字节
    - 布尔
      - boolean   2个字节
    - 字符
      - char      1个字节

6. 泛型只能是引用类型，不能是基本类型
基本类型： 
byte     -> Byte
short    -> Short
int      -> Integer
long     -> Long
float    -> Float
double   -> Double
char     -> Character
boolean  -> Boolean

7. 定义一个字符串是常量，不可改变

8. (int) num 强转数字 去掉小数