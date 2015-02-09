Android Native 几个需要注意的地方（一）
---
## OSX IDEA 下编译头文件
本人的环境为Yosemite，IDE 是`jetbrains`的`IntelliJ IDEA 14`。
### Class文件目录
在我们将 `native` 方法写入 `java`文件中。网上大多数的教程都是基于`eclipse`写的，这时他们会找你去`gen`文件夹找`class`文件，但`IDEA`生成的`class`并不在`gen`下，而是在`out`目录下。
### javah 命令
在进入`[project 要目录]/out/production/[project anme]`目录后，我们也不能直接使用 
``` shell
javah -jni xxx.xxx.xxx.[class name]
```
此时会报`class file for xxx.xxx.xxx not found`，这时我们需要加入classpatth 参数来指定，类似下述
``` shell
javah -classpath .:$ANDROID_SDK/platforms/android-19/android.jar -jni xxx.xxx.xxx.[class name]
```
`$ANDROID_SDK`这个环境变量指向的 SDK 根目录。