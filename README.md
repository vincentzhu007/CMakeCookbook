## CMakeCook笔记

### 第1章：hello world

1、使用对象库生成静态库和动态库：

```bash
➜  chapter01 git:(master) ✗ ls
CMakeFiles       hello_world      libmessage.a     libmessage.dylib
```
2、条件编译

变量的逻辑判断：
- 如果将逻辑变量设置为以下任意一种: 1 、 ON 、 YES 、 true 、 Y 或 非零数，则逻辑变量为 true 。
- 如果将逻辑变量设置为以下任意一种:0、 、NO、false、N、或者以 -NOTFOUND 为后缀，则逻辑变量为 false。

