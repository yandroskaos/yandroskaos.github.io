---
title: PEG parsers
description: PEG parsers
category: blog
---

When you think about parsing, the first thing it comes to mind is recursive descent (by hand) parsers and lex+yacc.

But things changed lately with [this paper](http://bford.info/pub/lang/peg.pdf).
Designing a new language is a challenge. There are lots of things involved and usually it is nice to have a short 
development cycle, but usually it is very hard to modify syntax.

Usually, you need to formalize syntax with a type-2 grammar (context-free) and then code it in yacc.
Then you need to get a hammer and hit your grammar spec until yacc does not cry anymore. Typically you end up with a semi-unfamiliar grammar which more or less parsers 
what you intended. Problem does not stop here as your beautiful, logical grammar productions which expressed concisely ideas and constructions in your language are 
not there anymore thanks to yacc and now you have to build an AST with data scathered all over the grammar.

That's why a lot of people defines informally its language syntax and then starts writing a recursive descent parser, which comes with its owns limitations (eg: if you have to deal with expressions and priorities, you may end up also with strange productions).

So what then?

PEG's are a powerful mechanism to get the best from the world of grammars: simple definition and they parse what you want

In the meantime, i've publish full source code in my github repo under [oolc](https://github.com/yandroskaos/oolc) so you can take a look.


