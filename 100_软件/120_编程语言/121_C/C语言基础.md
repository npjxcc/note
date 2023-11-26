## 常见的字符串处理函数

```c
/* #include <string.h> */

strlen()	//求字符串长度  (不计算\0)
//在字符串中使用反斜杠字符 '\' 和 NULL 字符 '\0' 时，应该注意它们的特殊性，不要将它们看作两个字符。因此，在计算该字符串长度时，strlen() 函数会把这两个字符都当作一个字符来处理，导致计算结果少了一个字符。

strcpy()	//字符串拷贝 

stract()	//字符串追加(连接) 

strcmp()	//字符串比较 


strncpy(p, p1, n) 	//复制指定长度字符串           把 p1 的n个字节拷贝到p里面

strncat(p, p1, n) 	//附加指定长度字符串 

strcasecmp(s1, s2)	//忽略大小写比较字符串

strncmp(p, p1, n) 	//比较指定长度字符串 

strchr(p, c) 		//在字符串中查找指定字符 

strstr(p, p1) 		//查找字符串 


/* #include <ctype.h> */

isalpha() 	//检查是否为字母字符 

isupper() 	//检查是否为大写字母字符 

islower() 	//检查是否为小写字母字符 

isdigit() 	//检查是否为数字 

```

