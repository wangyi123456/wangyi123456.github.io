# git and GitHub

![image-20201122210753590](https://i.loli.net/2020/11/22/G9oCa6ZkRhyO5Xq.png)

## 1 本地仓库

1. 初始化
2. 到需要管理的目录
3. git init 该目录
4. git status  看状况
5. add -> commit -m "name"

## 2 远程仓库

- git remote add origin https://github.com/songguoguo927/lunbotu.git

- ssh-keygen -t rsa -C “1369799720@qq.com”(随后将micro秘钥添加至该GitHub)
- git remote add origin git@github.com:*wangyi123456*/learngit
- git push -u origin main (第一次需要-u，以后就不需要了)
- **拉取远程仓库** git clone git@github.com:wangyi123456/python_data_structures_and_algorithms

## 3 错误处理

1. fatal: pathspec ‘hui’ did not match any files —> 文件名错误
2. 无法出现美元符号 —> ‘q’ 退出
3. 连接仓库出现错误的话 git remote rm origin



