### Shell：查找日期为某一天的文件
发表于: Linux, Shell, UNIX | 作者: 谋万世全局者
标签: Shell,文件,查找日期

#!/bin/sh
# The right of usage, distribution and modification is here by granted by the author.
# The author deny any responsibilities and liabilities related to the code.
#
OK=0
A=`find $1 -print`
if expr $3 == 1 >;/dev/null ; then M=Jan ; OK=1 ; fi
if expr $3 == 2 >;/dev/null ; then M=Feb ; OK=1 ; fi
if expr $3 == 3 >;/dev/null ; then M=Mar ; OK=1 ; fi
if expr $3 == 4 >;/dev/null ; then M=Apr ; OK=1 ; fi
if expr $3 == 5 >;/dev/null ; then M=May ; OK=1 ; fi
if expr $3 == 6 >;/dev/null ; then M=Jun ; OK=1 ; fi
if expr $3 == 7 >;/dev/null ; then M=Jul ; OK=1 ; fi
if expr $3 == 8 >;/dev/null ; then M=Aug ; OK=1 ; fi
if expr $3 == 9 >;/dev/null ; then M=Sep ; OK=1 ; fi
if expr $3 == 10 >;/dev/null ; then M=Oct ; OK=1 ; fi
if expr $3 == 11 >;/dev/null ; then M=Nov ; OK=1 ; fi
if expr $3 == 12 >;/dev/null ; then M=Dec ; OK=1 ; fi
if expr $3 == 1 >;/dev/null ; then M=Jan ; OK=1 ; fi
if expr $OK == 1 >; /dev/null ; then
ls -l --full-time $A 2>;/dev/null | grep "$M $4" | grep $2 ;
else
echo Usage: $0 path Year Month Day;
echo Example: $0 ~ 1998 6 30;
fi