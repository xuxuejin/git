## git add 命令

在使用 git add . 添加文件的时候，bash 会提示 “warning: LF will be replaced by CRLF in xxxx”，这是因为不同操作系统所使用的换行符不一致导致的。<br>三大主流操作系统的换行符：<br>Uinx/Linux采用换行符LF表示下一行（LF：LineFeed，中文意思是换行）；<br>Dos和Windows采用回车+换行CRLF表示下一行（CRLF：CarriageReturn LineFeed，中文意思是回车换行）；<br>Mac OS采用回车CR表示下一行（CR：CarriageReturn，中文意思是回车）。

通过使用 git config core.autocrlf 来显示当前你的Git中采取哪种对待换行符的方式，此命令会有三个输出，“true”，“false”或者“input”

为 true 时，Git 会将你 add 的所有文件视为文本文件，将结尾的 CRLF 转换为 LF，而 checkout 时会再将文件的 LF 格式转为 CRLF 格式。

为 false 时，line endings 不做任何改变，文本文件保持其原来的样子。 

为 input 时，add 时 Git 会把 CRLF 转换为 LF，而 check 时仍旧为 LF，所以 Windows 操作系统不建议设置此值。

执行 git config core.autocrlf false 设置，再执行 add 操作就没有问题了
