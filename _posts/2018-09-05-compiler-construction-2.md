---
title: PEG parsers
description: PEG parsers
category: blog
---

When you think about parsing, the first thing that comes to mind is recursive descent (by hand) parsers and lex+yacc.

But things changed lately with [this paper](http://bford.info/pub/lang/peg.pdf).
Designing a new language is a challenge. There are lots of things involved and usually it is nice to have a short 
development cycle, but usually it is very hard to modify syntax.

Usually, you need to formalize syntax with a type-2 grammar (context-free) and then code it in yacc.
Then you need to get a hammer and hit your grammar spec until yacc does not cry anymore. Typically you end up with a semi-unfamiliar grammar which more or less parses 
what you intended. Problem does not stop here as your beautiful, logical grammar productions which expressed concisely ideas and constructions in your language are 
not there anymore thanks to yacc and now you have to build an AST with data scathered all over the grammar.

That's why a lot of people defines informally its language syntax and then starts writing a recursive descent parser, which comes with its owns limitations.

So what then?

PEG's are a powerful mechanism to get the best from the world of grammars: simple definition and they parse what you want.
PEG's are basically recursive descent parsers expressed via parser combinators and where choice operation is **ordered**.

The fact that the choice operator has precedence means that a succesful branch of a rule, if successful, will discard any other.
This solves ambiguity in a typical CFG as it is resolved depending what branch is written first, which, is simple and allows the writer of the grammar to express its intentions.
If a branch does not successfully parse, parser backtracks and the next choice is tried.

There are drawbacks, such as the cost introduced by the bactracking on failed parsers, but it is not a big problem and can be minimized with memoization.
Given we are using parser combinators, any parser can be easily memoized with a Decorator pattern.

**In summary**, a PEG parser is a recursive descent mix between type 3 (regular expressions) parsers wich achieve type 2 behaviour (context-free) via calls (and recursive calls) between the parsers.
As a side note, it's possible even to go without a scanner as the type 3 expressions in the grammar will do. In this case, it's usual to see parsers to eat undesired input (such as whitespaces, comments, etc) at certain points in the grammar, which may be confusing, but on the other hand, brings powerful features and eases parsing by not having two distinct phases.


So if you want to play with pegs and parser combinators, i've shared source code in my github repo under [languages](https://github.com/yandroskaos/languages). There are a number of combinators for common type 3 and type 2 expressions as well as other *goodies* such as common parsers for more classic tokens and a whole class of *semantic* parsers to construct a syntax tree in a logical way.

Also, using those, i've described a language to describe the parsers (boot-strapped, self-reference... you know what i mean). It's also included as a demonstration of the use of the code, but can be used to have a nice parser generator which understands syntax like this:

```

GRAMMAR JavaLike

SETS
	Alpha		= ['a' .. 'z'] + ['A' .. 'Z'] + ['_'] ;
	DecimalDigit	= ['0' .. '9'] ;
	AlphaDigit	= Alpha + DecimalDigit ;
	WhiteSpaces     = [' ', NL, CR, TB] ;

COMMENTS
	SEPARATOR = WhiteSpaces ;
	COMMENT_1 = "/*" (!"*/" ANY)* "*/" ;
	COMMENT_2 = "//" (!NL ANY)* NL ;

SCANNER
	STRING_CTE = '"' (!'"' ANY)* '"' ;
	BOOL_CTE   = "true" | "false" ;
	CHAR_CTE   = "'" ANY "'" ;
	INT_CTE    = DecimalDigit+ ;
	REAL_CTE   = DecimalDigit* '.' DecimalDigit+ ;
	IDENTIFIER = Alpha AlphaDigit* ;
	
PARSER
	r_type = IDENTIFIER "[]"* ;

	r_brackets = '[' r_expression ']' ;
	r_call     = '(' (r_expression (',' r_expression)*) ? ')' ;
	r_slot     = IDENTIFIER (r_call | r_brackets)* ;
	r_slots    = r_slot ('.' r_slot)* ;
	
	r_memory = "new" IDENTIFIER r_call? | "delete" r_expression;
	
	r_expression = r_ternary;
	r_ternary       = r_expr_level_0 ('?' r_expr_level_0 ':' r_expr_level_0) ? ;
	r_expr_level_0  = r_expr_level_1  (r_op_assignment     r_expr_level_1 )* ;
	r_expr_level_1  = r_expr_level_2  (r_op_logical_or     r_expr_level_2 )* ;
	r_expr_level_2  = r_expr_level_3  (r_op_logical_and    r_expr_level_3 )* ;
	r_expr_level_3  = r_expr_level_4  (r_op_bitwise_in_or  r_expr_level_4 )* ;
	r_expr_level_4  = r_expr_level_5  (r_op_bitwise_ex_or  r_expr_level_5 )* ;
	r_expr_level_5  = r_expr_level_6  (r_op_bitwise_and    r_expr_level_6 )* ;
	r_expr_level_6  = r_expr_level_7  (r_op_equality       r_expr_level_7 )* ;
	r_expr_level_7  = r_expr_level_8  (r_op_relational     r_expr_level_8 )* ;
	r_expr_level_8  = r_expr_level_9  (r_op_shift          r_expr_level_9 )* ;
	r_expr_level_9  = r_expr_level_10 (r_op_additive       r_expr_level_10)* ;
	r_expr_level_10 = r_primary       (r_op_multiplicative r_primary      )* ;
	r_primary = '(' r_expression ')' | r_constant | r_op_unary r_primary | r_memory | r_slots ;

	r_constant = BOOL_CTE | CHAR_CTE | INT_CTE | REAL_CTE | STRING_CTE;

	r_variable = IDENTIFIER ('=' r_expression)? ;
	r_declaration = r_type r_variable (',' r_variable)* ;

	r_op_unary          = "++" | "--" | "-"  | "~" | "!";
	r_op_multiplicative = "*"  | "/"  | "%";
	r_op_additive       = "+"  | "-";
	r_op_shift          = "<<" | ">>";
	r_op_relational     = "<"  | ">"  | "<=" | ">=";
	r_op_equality       = "==" | "!=";
	r_op_bitwise_and    = "&";
	r_op_bitwise_ex_or  = "^";
	r_op_bitwise_in_or  = "|";
	r_op_logical_and    = "&&";
	r_op_logical_or     = "||";
	r_op_assignment     = "+=" | "-=" | "*=" | "/=" | "%=" | "&=" | "^=" | "|=" | "<<=" | ">>=" | "=" ;

	r_statement = 
		'{' r_statement+ '}'                                                                          |
		"write"  r_expression  ';'                                                                    |
		"read"   r_slots       ';'                                                                    |
		"return" r_expression? ';'                                                                    |
		"if"    '(' r_expression ')' r_statement ("else" r_statement)?                                |
		"while" '(' r_expression ')' r_statement                                                      |
		"do" r_statement "while" '(' r_expression ')' ';'                                             |
		"for" '(' (r_declaration | r_expression)? ';' r_expression? ';' r_expression? ')' r_statement |
		"break"       ';'                                                                             |
		"continue"    ';'                                                                             |
		r_declaration ';'                                                                             |
		r_expression  ';'                                                                             ;

	r_code = r_statement* ;

	r_argument = r_type IDENTIFIER ;
	
	r_arguments = r_argument (',' r_argument)* ;

	r_prototype = r_type IDENTIFIER '(' r_arguments? ')' ';' ;
	
	r_interface = "interface" IDENTIFIER '{' r_prototype* '}' ;

	r_static = "static"? ;

	r_access = ("private" | "protected" | "public")?;

	r_implements = "implements" IDENTIFIER (',' IDENTIFIER)* ;
	
	r_extends = "extends" IDENTIFIER ;

	r_attribute = IDENTIFIER ('=' r_expression)? ;

	r_attribute_list = r_attribute (',' r_attribute)* ;
	
	r_attributes = r_static r_access r_type r_attribute_list ';';

	r_method = r_static r_access r_type IDENTIFIER '(' r_arguments? ')' '{' r_code '}';

	r_constructor = IDENTIFIER '(' r_arguments? ')' '{' r_code '}';

	r_class = "class" IDENTIFIER r_extends? r_implements? '{' (r_attributes | r_constructor | r_method)* '}';

	r_program = (r_class | r_interface)+ EOI ;

	start = r_program;

```

Hope you enjoy it!


