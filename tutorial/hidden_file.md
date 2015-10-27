---
layout: default
---

This tutorial outlines the steps to create a module with JReFrameworker to modify the behavior of the Java runtime's `java.io.File` class to return false if the file name is "secretFile" regardless if the file actually exists or not.

## Creating a new module

Each "module" consists of an Eclipse JReFrameworker project.  A module consists of annotated Java source code for one or more Java classes, which define how the runtime environment should be modified. A module may also contain test code that uses the runtime APIs that will be modified.  The test code can be used to execute and debug the module in the modified as well as original runtime environments.

To create a new attack module, navigate to `File` &gt; `New` &gt; `Other...` &gt; `JReFrameworker` &gt; `JReFrameworker Runtime Project`.

<center>
![New Modue](/JReFrameworker/tutorial/hidden_file_images/NewProject.png)
</center>

Enter a new project name for the module.

<center>
![Module Name](/JReFrameworker/tutorial/hidden_file_images/NewProjectName.png)
</center>

## Adding test logic

Next let's add some test code that will interact with the `java.io.File` API so that we can effectively test the modified runtime. The following `Test` class contains a main method that creates a `File` named "secretFile" and writes the string "blah" to the file.  After the file is written, the existence of the file is printed to the console.  Finally, the file is deleted.

	import java.io.File;
	import java.io.FileWriter;
	import java.io.IOException;
	
	public class Test {
	
		public static void main(String[] args) throws IOException {
			File testFile = new File("secretFile");
			FileWriter fw = new FileWriter(testFile);
			fw.write("blah");
			fw.close();
			System.out.println("Secret File Exists: " + testFile.exists());
			testFile.delete();
		}
	
	}
	
<center>
![Test Logic](/JReFrameworker/tutorial/hidden_file_images/TestLogic.png)
</center>

In an unmodified runtime, the print out should return "true" assuming the file could be written.  In the event that a file could not be written and exception will be thrown causing stack trace to be written to the output.

Our goal is to modify the runtime so that the print out reads "false" if and only if the `File` object's name is "secretFile".

## JReFrameworker Annotations
Coming soon...