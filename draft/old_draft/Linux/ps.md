# ps
<div id="_ps"></div>
	语　　法：ps [-aAcdefHjlmNVwy][acefghLnrsSTuvxX][-C &lt;指令名称&gt;][-g &lt;群组名称&gt;][-G &lt;群组识别码&gt;][-p &lt;程序识别码&gt;][p &lt;程序识别码&gt;][-s &lt;阶段作业&gt;][-t &lt;终端机编号&gt;][t &lt;终端机编号&gt;][-u &lt;用户识别码&gt;][-U &lt;用户识别码&gt;][U &lt;用户名称&gt;][-&lt;程序识别码&gt;][--cols &lt;每列字符数&gt;][--columns &lt;每列字符数&gt;][--cumulative][--deselect][--forest][--headers][--help][-- info][--lines &lt;显示列数&gt;][--no-headers][--group &lt;群组名称&gt;][-Group &lt;群组识别码&gt;][--pid &lt;程序识别码&gt;][--rows &lt;显示列数&gt;][--sid &lt;阶段作业&gt;][--tty &lt;终端机编号&gt;][--user &lt;用户名称&gt;][--User &lt;用户识别码&gt;][--version][--width &lt;每列字符数&gt;]
	参　　数： 
	-a 　显示所有终端机下执行的程序，除了阶段作业领导者之外。 
	a 　显示现行终端机下的所有程序，包括其他用户的程序。 
	-A 　显示所有程序。 
	-c 　显示CLS和PRI栏位。 
	c 　列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示。 
	-C&lt;指令名称&gt; 　指定执行指令的名称，并列出该指令的程序的状况。 
	-d 　显示所有程序，但不包括阶段作业领导者的程序。 
	-e 　此参数的效果和指定A参数相同。 
	e 　列出程序时，显示每个程序所使用的环境变量。 
	-f 　显示UID,PPIP,C与STIME栏位。 
	f 　用ASCII字符显示树状结构，表达程序间的相互关系。 
	-g&lt;群组名称&gt; 　此参数的效果和指定-G参数相同，当亦能使用阶段作业领导者的名称来指定。 
	g 　显示现行终端机下的所有程序，包括群组领导者的程序。 
	-G&lt;群组识别码&gt; 　列出属于该群组的程序的状况，也可使用群组名称来指定。 
	h 　不显示标题列。 
	-H 　显示树状结构，表示程序间的相互关系。 
	-j或j 　采用工作控制的格式显示程序状况。 
	-l或l 　采用详细的格式来显示程序状况。 
	L 　列出栏位的相关信息。 
	-m或m 　显示所有的执行绪。 
	n 　以数字来表示USER和WCHAN栏位。 
	-N 　显示所有的程序，除了执行ps指令终端机下的程序之外。 
	-p&lt;程序识别码&gt; 　指定程序识别码，并列出该程序的状况。 
	p&lt;程序识别码&gt; 　此参数的效果和指定-p参数相同，只在列表格式方面稍有差异。 
	r 　只列出现行终端机正在执行中的程序。 
	-s&lt;阶段作业&gt; 　指定阶段作业的程序识别码，并列出隶属该阶段作业的程序的状况。 
	s 　采用程序信号的格式显示程序状况。 
	S 　列出程序时，包括已中断的子程序资料。 
	-t&lt;终端机编号&gt; 　指定终端机编号，并列出属于该终端机的程序的状况。 
	t&lt;终端机编号&gt; 　此参数的效果和指定-t参数相同，只在列表格式方面稍有差异。 
	-T 　显示现行终端机下的所有程序。 
	-u&lt;用户识别码&gt; 　此参数的效果和指定-U参数相同。 
	u 　以用户为主的格式来显示程序状况。 
	-U&lt;用户识别码&gt; 　列出属于该用户的程序的状况，也可使用用户名称来指定。 
	U&lt;用户名称&gt; 　列出属于该用户的程序的状况。 
	v 　采用虚拟内存的格式显示程序状况。 
	-V或V 　显示版本信息。 
	-w或w 　采用宽阔的格式来显示程序状况。　 
	x 　显示所有程序，不以终端机来区分。 
	X 　采用旧式的Linux i386登陆格式显示程序状况。 
	-y 　配合参数-l使用时，不显示F(flag)栏位，并以RSS栏位取代ADDR栏位　。 
	-&lt;程序识别码&gt; 　此参数的效果和指定p参数相同。 
	--cols&lt;每列字符数&gt; 　设置每列的最大字符数。 
	--columns&lt;每列字符数&gt; 　此参数的效果和指定--cols参数相同。 
	--cumulative 　此参数的效果和指定S参数相同。 
	--deselect 　此参数的效果和指定-N参数相同。 
	--forest 　此参数的效果和指定f参数相同。 
	--headers 　重复显示标题列。 
	--help 　在线帮助。 
	--info 　显示排错信息。 
	--lines&lt;显示列数&gt; 　设置显示画面的列数。 
	--no-headers 　此参数的效果和指定h参数相同，只在列表格式方面稍有差异。 
	--group&lt;群组名称&gt; 　此参数的效果和指定-G参数相同。 
	--Group&lt;群组识别码&gt; 　此参数的效果和指定-G参数相同。 
	--pid&lt;程序识别码&gt; 　此参数的效果和指定-p参数相同。 
	--rows&lt;显示列数&gt; 　此参数的效果和指定--lines参数相同。 
	--sid&lt;阶段作业&gt; 　此参数的效果和指定-s参数相同。 
	--tty&lt;终端机编号&gt; 　此参数的效果和指定-t参数相同。 
	--user&lt;用户名称&gt; 　此参数的效果和指定-U参数相同。 
	--User&lt;用户识别码&gt; 　此参数的效果和指定-U参数相同。 
	--version 　此参数的效果和指定-V参数相同。 
	--widty&lt;每列字符数&gt; 　此参数的效果和指定-cols参数相同。
　　
　　
