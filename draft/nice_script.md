# nice script

直接调用google-translate-cli（免安装）在命令行下做翻译：

gawk "$(curl -Ls git.io/google-translate-cli)" {sv=zh} "God morgon."