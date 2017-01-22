(3)将当前目录下的所有文件名由GBK转为UTF8 
  convmv -r -f GBK -t UTF-8 --notest --nosmart *
 
附：iconv是文件内容编码转换工具，把gbk编码的a.txt文件转换成utf8编码的b.txt 
iconv -f GBK -t UTF-8 a.txt -o b.txt 
