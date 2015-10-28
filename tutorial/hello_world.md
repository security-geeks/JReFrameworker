---
layout: default
---

Take a look at the following code. You've probably even written this exact snippet before. What is the output?

	public class Test {
	
		public static void main(String[] args) {
			System.out.println("Hello World!");
		}
		
	}

Would you be suprised if the output was `!dlroW olleH` and not `Hello World!`?  How could this be possible?  There are no tricks in this program.  It's the standard hello world program you've seen a hundred times before.

This behavior is possible if the underlying Java libraries (the Java Runtime Environment) were modified. Since making bytecode manipulations manually would require a deep understanding of the Java bytecode instructions as well as recomputing stack frame sizes to account for instruction changes. Some tools have used intermediate representations (IRs) that can dissassemble and reassemble bytecode, but most developers are not familiar with various IRs and there is poor tool support for serious development tasks.  Worse yet, the ["write once, run anywhere"](https://en.wikipedia.org/wiki/Write_once,_run_anywhere) motto of managed languages is violated when dealing with runtime libraries, forcing the developer to to rewrite modifications for each target platform.

JReFrameworker is a tool that allows you to write simple Java source code with a set of annotations that define how to rewrite the underlying runtime library.  The primary purpose of the tool is for developing offensive security "modules", but there are several other reasons you might want to rewrite the runtime environment.  The code below is all that is needed to modify the runtime to make our hello world application print backwards.  For more details on developing modules see the next [tutorial](/JReFrameworker/tutorial/hidden_file).

	package java.io;
	
	import jreframeworker.annotations.methods.MergeMethod;
	import jreframeworker.annotations.types.MergeType;
	
	@MergeType
	public class BackwardsPrintStream extends PrintStream {
	
		public BackwardsPrintStream(OutputStream os) {
			super(os);
		}
		
		@MergeMethod
		@Override
		public void println(String str){
			StringBuilder sb = new StringBuilder(str);
			super.println(sb.reverse().toString());
		}
	
	}

The JReFrameworker module developed for this tutorial can be downloaded [here](/JReFrameworker/module/HiddenFile.zip).