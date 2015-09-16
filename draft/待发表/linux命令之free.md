# Linux命令之free

free是显示的当前内存的使用,-m的意思是M字节来显示内容。

	$ free -m
	             total       used       free     shared    buffers     cached
	Mem:         1002        769        232          0         62        421
	-/+ buffers/cache:        286        715
	Swap:         1153          0       1153

### Mem行 

	total：表示物理内存总量(total = used + free)
	used：表示总计分配给缓存（包含buffers 与cache ）使用的数量，但其中可能部分缓存并未实际使用。
	free：未被分配的内存。
	shared：共享内存，一般系统不会用到，这里也不讨论。
	buffers：系统分配但未被使用的buffers 数量。
	cached：系统分配但未被使用的cache 数量。

### (-/+ buffers/cache)

	(-buffers/cache) used内存数：286M (指的第一部分Mem行中的used - buffers - cached)  
	(+buffers/cache) free内存数: 715M (指的第一部分Mem行中的free + buffers + cached)  

buffers是指用来给块设备做的缓冲大小，他只记录文件系统的metadata以及 tracking in-flight pages.
cached是用来给文件做缓冲。
那就是说：buffers是用来存储，目录里面有什么内容，权限等等。
而cached直接用来记忆我们打开的文件。  

### 交换区的信息

buffer/cached是为了提高程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。 所以,以应用来看看,以(-/+ buffers/cache)的free和used为主.


	语　　法： free [-bkmotV][-s <间隔秒数>]
	
	补充说明：free指令会显示内存的使用情况，包括实体内存，虚拟的交换文件内存，共享内存区段，以及系统核心使用的缓冲区等。
	
	参　　数：
	-b 　以Byte为单位显示内存使用情况。
	-k 　以KB为单位显示内存使用情况。
	-m 　以MB为单位显示内存使用情况。
	-o 　不显示缓冲区调节列。
	-s<间隔秒数> 　持续观察内存使用状况。
	-t 　显示内存总和列。
	-V 　显示版本信息。