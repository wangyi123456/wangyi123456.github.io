# win 10 下 darknet配置

## 1 darknet本身

1. 下载软件和驱动 链接：https://pan.baidu.com/s/19vOGqaqEZeMc2Q32yZOb7w 提取码：rf48![image-20201203182652996](win%2010%20%E4%B8%8B%20darknet%E9%85%8D%E7%BD%AE.assets/image-20201203182652996.png)

2. ```java
   添加open cv 的系统环境变量 OpenCV_DIR` = `C:\opencv\build
   ```

3. 双击 cuda 安装,勾选添加环境变量,然后将cudnn 粘贴到cuda下面:![image-20201203183528790](win%2010%20%E4%B8%8B%20darknet%E9%85%8D%E7%BD%AE.assets/image-20201203183528790.png)

4. 从码云下载官方源码,使用cmake配置生成vs项目:

   ![image-20201203183933966](win%2010%20%E4%B8%8B%20darknet%E9%85%8D%E7%BD%AE.assets/image-20201203183933966.png)

   5. 生成解决方案,会打开vs,然后build,会产生.EXE文件,在对应生成的x64目录文件夹下粘贴已有的weights文件,就可以通过命令行运行了

## 2 使用显卡和open cv

1.  vs code 打开 darknet\build\darknet 下的 project  文件,将cuda版本改为系统版本(两处,一处在文件末尾)![image-20201203181850483](win%2010%20%E4%B8%8B%20darknet%E9%85%8D%E7%BD%AE.assets/image-20201203181850483.png)
2. 用vs 打开项目,改为 x64 release 模式,添加open cv 头文件,库目录,附加包含目录,并将 cuda驱动改为相应版本(去英伟达官网查看自己的显卡对应哪个.1660ti对应 61)
3. 如果想要查看是否使用显卡可以用 nvida-smi 同时运行darknet看显卡是否工作

> **问题** 
>
> 1, 可否在第一次运行生成的时候就把OpenCV和cuda依赖填好
>
> 2, 上图YOLO和darknet是两个都要修改吗?
>
> 3, 怎么以项目模式打开vs来着

## 3 集成qt

### darknet文件依赖

![image-20201203175016043](C:%5CUsers%5C37779%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201203175016043.png)

两个dll 在 `C:\Users\37779\Desktop\des\my codes\python\darknet\build\darknet\x64`

hpp 在在 darknet\include文件夹下面

### cmake 文件编写

1. 找到open cv 头文件

   ```makefile
   find_package(OpenCV REQUIRED)
   include_directories(${OpenCV_INCLUDE_DIRS})
   ```

2. 动态链接

```makefile
target_link_libraries(darknet3 PRIVATE ${OpenCV_LIBS})
target_link_libraries(darknet3 PRIVATE ${PROJECT_SOURCE_DIR}/yolo_cpp_dll.lib)
```

3. 在 add_executable 引入 yolo_v2_class.hpp

### 编写例子

1. 定义槽函数

2. 在relese模式下运行,会在项目目录.. 生成release文件夹,内有项目.exe文件,直接运行会报错

3. 在release下建立 build 文件夹,内置:![image-20201203180850209](C:%5CUsers%5C37779%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201203180850209.png)

4. 添加链接库依赖:![image-20201203181040091](C:%5CUsers%5C37779%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201203181040091.png)

   除了open cv 的基本都在 `\darknet\build\darknet\x64`文件夹下面

