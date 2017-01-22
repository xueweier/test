# linux命令之colrm

列删除过滤器. 这个工具将会从文件中删除指定的列(列中的字符串)并写到文件中。 

	stdout. colrm 2 4 <filename将会删除filename文件中每行的第2到第4列之间的所有字符.
	如果这个文件包含tab和不可打印字符, 那将会引起不可预期的行为. 在这种情况 下, 应该通过管道的手段使用expand和unexpand来预处理colrm. 