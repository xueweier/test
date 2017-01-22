tar打包的时候显示

tar (child): Cannot connect to SQliteManager-1.2.4.tar.gz?r=http: resolve failed

gzip: stdin: unexpected end of file
tar: Child returned status 128
tar: Error is not recoverable: exiting now


原因是
The reason for the error you are seeing can be found in the GNU tar documentation:

If the archive file name includes a colon (‘:’), then it is assumed to be a file on another machine[...]


http://unix.stackexchange.com/questions/13377/tar-extraction-depends-on-filename