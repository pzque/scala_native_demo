# Sample

# Structure

"Sample" directory is a Intellij IDEA project built with sbt.

"SampleSubc" is a Clion project built with cmake.

"lib" contains libraries generated by the SampleSubc project. 

# Workflow
## 1. 打开项目
使用idea打开Sample项目，clion打开SampleSubc项目。

## 2. 创建含有native方法的scala类
在idea内，/src/main/scala/文件夹下创建一个含有native方法的scala类：

例如
```scala
class OwnMath {
  @native def add(a:Double,b:Double):Double
}
```

## 3. 编译scala类
选择compile配置，如下图，注意是compile不是run。

![](https://coding.net/u/zeta159/p/Sample/git/raw/master/doc/sbt-task-icon.jpg)
 
然后点击旁边的三角符号运行compile任务（它是一个sbt task）进行编译。

编译时构建脚本会调用`javah`命令生产相应的.h文件到“SampleSubc/OwnMath.h”内，并且自动生成相应的.cpp文件，在CMakeLists.txt里加入一个新的动态库target。

## 4. 编译c++动态库
然后转到clion，依次点击菜单栏的Run->Build选项进行cpp工程的编译，编译完成后，动态库会生成到`lib`目录内。

## 5.在scala内使用动态库
动态库编译完成就可以在scala项目内任意使用了。

这是因为在jvm的启动参数里设置了`-Djava.library.path=/Users/pcz/IdeaProjects/Sample/lib`选项。

设置方式是点击“Edit Configurations...”

![](https://coding.net/u/zeta159/p/Sample/git/raw/master/doc/sbt-task-icon.jpg)

然后进入如下的页面：

![](https://coding.net/u/zeta159/p/Sample/git/raw/master/doc/vm-option.jpg)

在自己使用的时候需要将其中的路径改成自己的lib路径。
