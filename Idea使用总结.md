## 安装 Maven
* 下载 Maven [官网地址](http://maven.apache.org/download.cgi)
* 配置环境[Ref:CSDN](https://blog.csdn.net/fenglailea/article/details/69500318)
```
# 打开 vim
vim ~/.bash_profile
# 加入环境变量
export MAVEN_HOME=/Users/fred/Downloads/apache-maven-3.6.1
export M2_HOME=$MAVEN_HOME
export PATH=$PATH:$MAVEN_HOME/bin
# 配置生效
source ~/.bash_profile
# 验证mvn安装
mvn -v
```
