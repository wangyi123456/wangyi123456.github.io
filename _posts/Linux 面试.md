# Linux 面试

## 1 架构

![image-20201114151533452](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114151533452.png)

## 2 基本指令(ls,cat,vim,cp,mv,sort,tail,head)

1. **基本命令 ls，cat，vim，cp和mv**
2. **sort命令可以实现对文件进行排序**。

- 正序排序：**sort -n test.txt**
- 反序排序：**sort –nr test.txt**

3. **tail 和 head 命令**

- **tail –n 2 file.log** 可以查看文件的最后2行。
- **tail –f file.log**可以实时查看文件的后边追加的部分。
- **head –n 2 file.log**可以查看文件的开始2行。



## 3 如何查找一个特定的文件(find)

![image-20201114152624277](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114152624277.png)

## 4 检索文件内容(grep)

![image-20201114154529410](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114154529410.png)

## 5 对文件内容做统计(awk)

![image-20201114155727659](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114155727659.png)

假设我们现在有一个文件，里边内容有三行，如下所示，我们来一起做几个简单的awk统计吧。

- **去掉第一列**
- **对第一列求和**
- **去掉列数不为3的列**

![img](https://uploadfiles.nowcoder.com/images/20191124/5459305_1574592427900_D15C4A3ECD662B73D0D2B4FA766E738D)

**汇总**

![image-20201114155545623](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114155545623.png)

## 6 批量替换文本内容(sed)

![image-20201114160259436](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201114160259436.png)

## 7 进程内容查看和监视(ps top)

**ps和top命令的区别：**

- ps看到的是命令执行瞬间的进程信息，而top可以持续的监视。
- ps只是查看进程，而top还可以监视系统性能，如平均负载，cpu和内存的消耗。
- top可以操作进程,如改变优先级(命令r)和关闭进程(命令k)。
- ps主要是查看进程的，关注点在于查看需要查看的进程。
- top主要看cpu，内存使用情况，及占用资源最多的进程由高到低排序，关注点在于资源占用情况。