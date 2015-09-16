### 实现分割文件的Shell脚本
发表于: Linux, Shell, UNIX | 作者: 谋万世全局者
标签: Shell,分割文件,脚本

#!/bin/bash

if [ $# -ne 2 ]; then
echo 'Usage: split file size(in bytes)'
exit
fi

file=$1
size=$2

if [ ! -f $file ]; then
echo "$file doesn't exist"
exit
fi

#TODO: test if $size is a valid integer

filesize=`/bin/ls -l $file | awk '{print $5}'`
echo filesize: $filesize

let pieces=$filesize/$size
let remain=$filesize-$pieces*$size
if [ $remain -gt 0 ]; then
let pieces=$pieces+1
fi
echo pieces: $pieces

i=0
while [ $i -lt $pieces ];
do
echo split: $file.$i:
dd if=$file of=$file.$i bs=$size count=1 skip=$i
let i=$i+1
done

echo "#!/bin/bash" >; merge

echo "i=0" >;>; merge
echo "while [ $i -lt $pieces ];" >;>; merge
echo "do" >;>; merge
echo " echo merge: $file.$i" >;>; merge
echo " if [ ! -f $file.$i ]; then" >;>; merge
echo " echo merge: $file.$i missed" >;>; merge
echo " rm -f $file.merged" >;>; merge
echo " exit" >;>; merge
echo " fi" >;>; merge
echo " dd if=$file.$i of=$file.merged bs=$size count=1 seek=$i" >;>; merge
echo " let i=$i+1" >;>; merge
echo "done" >;>; merge
chmod u+x merge'