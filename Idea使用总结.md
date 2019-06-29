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

## 快捷键
* 首字母缩写
* psvm #main方法
* sout #等同于syso


## poi包
* apache的office处理包
### 报错
#### XSSFCell.CELL_TYPE_STRING 找不到[Ref:CSDN](https://blog.csdn.net/weixin_43209201/article/details/86519522)
* 原因: apache官方3.1.5beta3改动
* 解决: 用 CellType.STRING/CellType.NUMERIC 替代

#### Exception in thread "main" java.io.IOException: Zip bomb detected! The file would exceed the max. ratio of compressed file size to the size of the expanded data.
...... MIN_INFLATE_RATIO: 0.010000, Entry: xl/styles.xml[Ref:520mwx](https://www.520mwx.com/view/33447)
* 原因:  延迟解析比率
* 解决
```
// 延迟解析比率
ZipSecureFile.setMinInflateRatio(-1.0d);
workbook = new XSSFWorkbook(file.getInputStream());
```

#### 日期导入时某一段为43965之类的数字 (Ref:CSDN)(https://blog.csdn.net/weixin_39800144/article/details/78668579)
* 43965是对于1900年1月1日的日子数，转换为获取到1900年1月1日的 date 后累加日期再 DateFormat
