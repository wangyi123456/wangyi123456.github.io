# Git 使用教程
> 这里我写的相当简略，没有任何学习或者实战基础的人可能会觉得不知所云，但如果你真的去学了，这块儿便有查漏补缺的作用
## 1.1 必须记住的一些命令
1. cd "d:\\fkg\goij"
1. git add readme.txt 
2. git commit -m"给这次改动起个名字" 
3. $ git log --pretty=oneline (add zhushi as line)
4. $ git reset --hard 8d027 (or HEAD^n)
5. git checkout -- readme.txt (回到最近一次的修改)

## 1.2 连接远程仓库
- ssh-keygen -t rsa -C "1369799720@qq.com"(随后将micro秘钥添加至该GitHub)
- git remote add origin git@github.com:*wangyi123456*/learngit
- git push -u origin master (第一次需要-u，以后就不需要了)
- **拉取远程仓库**：$ git clone git@github.com:wangyi123456/python_data_structures_and_algorithms


## 1.3 常见错误的更正方法
1. fatal: pathspec 'hui' did not match any files ---> 文件名错误
2. 无法出现美元符号 ---> 'q' 退出
