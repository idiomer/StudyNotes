# linux学习笔记

## 1.实用命令

### date 日期
``` shell
date -d "1 days ago ${today}" +%Y-%m-%d
```

### pwd 获取当前执行脚本的绝对路径
``` shell
scriptpath=$(cd `dirname $0`; pwd)
```

### mail 发邮件
``` shell
echo "backup failed! Please check it!" | mail -s "backup failed!" ${EMAIL}
echo "你好！我是正文" | mail --content-type="text/html;charset=utf-8" -s "backup failed!" ${EMAIL}

cat /home/tt/code/kanzhihu.com/data/quesion_detail.txt| sort -rnk 2 | head -n 100 |awk -F "\t" '{print "https://www.zhihu.com/question/"$0}' | iconv -cs -f "utf-8" -t "gb2312" | mail --content-type="text/html;charset=gb2312" -s "[`date +%Y-%m-%d`]问题列表top100" ${EMAIL}
```

### grep + find
``` shell
grep 'codec = '  `find ~/code -name '*.py'`
```

### sort 排序之第3列数值降序
``` shell
sort -t $'\t' -rnk 3 a.txt  #r:reversed, n:number, k:k-th
```

### shuf 洗牌之随机获取100行
``` shell
shuf -n 100 a.txt > a.100.txt
```

### uniq 去重之并、交、并-交
``` shell
cat a.txt | sort | uniq > b.txt #去重(并)
cat a.txt | sort | uniq -d > b.txt #只显示重复行(交)
cat a.txt | sort | uniq -u > b.txt #只显示不重复行(并-交)
```

### for 快速生成1~100
``` shell
for ((i=1;i<=100;++i)) do echo $i; done
```

### 整齐打印json
``` shell
echo "{"a":1, "b":2}" | python -m json.tool
```

### rsync/scp 远程同步
``` shell
rsync -avrp -P -e 'ssh -p 36000' ~/tmp/  username@host:~/tmp/
scp -P 36000 -r  ~/tmp/  username@host:~/tmp/
```

### stdbuf 实时重定向输出:所有输出同时在屏幕和log文件中实时输出
```shell
stdbuf -o0 python tmp.py 2>&1 |tee tmp.log
```

### uniq + sort 双射去重
``` shell
echo -e "1 to 2\n1 to 3\n11 to 3\n111 to 333" | sort -k 3 |  uniq -f 2 -u |awk '{print $3" "$2" "$1}' | sort -k 3 |  uniq -f 2 -u | awk '{print $3" "$2" "$1}' #1,3列双射
```

### split 大文件分割
```
zcat /media/tt/SSD1T/datamining.ym/dmuser/tzhou/test_dir/appUsers/haitao/total/part-*  | awk 'length($0)==14||length($0)==15{print}' | split - -b 102400 -d -a 4 imei_
# - 文件为标准输入
# -b 按Bytes分割，100KB每个文件（-l:按固定行数分割  -n:按固定输出文件个数分割）
# -d 后缀或前缀为数字
# -a append后缀，后缀长度为4，固定前缀为imei_
```

### tar 打包解包
```shell
tar -cvzf ./output.tar.gz /etc/       #打包压缩：c打包，v详情，z以gzip压缩，f输出文件
cd /etc && tar -xvzf ./output.tar.gz  #解压缩包：x解包
# z --> j 压缩方式由gzip换成bzip2
```

### awk 字典计数
``` shell
cat a.txt | awk -F "\t" '{++d[$1]}END{for (i in d) print i,d[i]}' | sort -rnk 2
```

### awk 按列(key)分割成多个文件
```shell
awk '{print $0 >> $1".txt"}' largeFile.txt
```

### tr 大小写转换(sed 并删除“-”)
``` shell
cat idfa_origin.txt | tr '[A-Z]' '[a-z]' | sed 's/-//g' > idfa.txt
```

### sed截取正则字符串
``` shell
# sed 's/old/new/g' a.txt
echo "sdf=1&at=15&abs=5" | sed 's/.*&\(at=[0-9]*\)&.*/\1/g'
# s=15
zcat ~/tmp/part* | sed 's/.*&\(at=[0-9]\+\)&.*/\1/g'
# note: "(", ")" and "+" should be escaped, while "[" and "]" keep themselves
```






