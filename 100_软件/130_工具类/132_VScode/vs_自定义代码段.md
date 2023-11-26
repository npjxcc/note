## NPJX\_general\_code\_fragment.code-snippets

{
	// Place your 全局 snippets here. Each snippet is defined under a snippet name and has a scope, prefix, body and 
	// description. Add comma separated ids of the languages where the snippet is applicable in the scope field. If scope 
	// is left empty or omitted, the snippet gets applied to all languages. The prefix is what is 
	// used to trigger the snippet and the body will be expanded and inserted. Possible variables are: 
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. 
	// Placeholders with the same ids are connected.
	// Example:
	// "Print to console": {
	// 	"scope": "javascript,typescript",
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }

	"line comment": {
		"prefix": "/*",
		"body": [
			"/* $1 */"
		],
		"description": "单行注释"
	},

	"area comment": {
		"prefix": "/**",
		"body": [
			"/*********************************************** $1 ***********************************************/"
		],
		"description": "区域分割注释"
	},

	"JL LOG": {
		"prefix": "NPJX_JL_LOG",
		"body": [
			"VTRACE(\"[NPJX] $1\");"
		],
		"description": "JL单行注释"
	},

	"Print to add line": {
		"prefix": "/CHANGE",
		"body": [
		
			"// Editor: NPJX, Time:$CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND"
		],
		"description": "在sdk中添加私人 修改"
	},

	"Print to add info": {
		"prefix": "/START",
		"body": [
			"// ============== Editor: NPJX, start block modify, Time:$CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND =================",
		],
		"description": "在sdk中添加私人 修改"
	},

	"Print to console": {
		"prefix": "/END",
		"body": [
			"// ============== Editor: NPJX, end block modify, Time:$CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND =================",
	
		],
		"description": "在sdk中添加私人 修改"
	}
}
## c.json

{
// Place your snippets for c here. Each snippet is defined under a snippet name and has a prefix, body and
// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the
// same ids are connected.

// Example:
// "Print to console": {
	/*
		就是代码块的作用文件类型，这里我们可以指定文件类型，
		多种类型之间用逗号隔开，
	*/

//"scope": "javascript,typescript",	//作用文件类型
// 	"prefix": "log",				// 触发代码块的字符串
// 	"body": [						//代码块的主体内容
// 		"console.log('$1');",
// 		"$2"
// 	],
// 	"description": "Log output to console"
// }

"Print Hello World": {
		"prefix": "NPJX_TEST_INIT_C",
		"body": [
			""
			"#include <stdio.h>",
			"",
			"int main(void)",
			"{"
			"	printf(\"hello world!\");",
			"	$1"
			"	return 1;",
			"}",
		],
		"description": "C test code snippet initialization"
},


"TODO": {
	"prefix": "NPJX_TODO",
	"body": [
		"/* TODO:[NPJX]$1 */",
	],
	"description": "Log output to console"
},
}
