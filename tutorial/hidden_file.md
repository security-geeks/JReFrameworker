---
layout: default
---

This tutorial outlines the steps to create a module with JReFrameworker to modify the behavior of the Java runtime's `java.io.File` class to return false if the file name is "secretFile" regardless if the file actually exists or not.

## Creating a New Module

Each "module" consists of an Eclipse JReFrameworker project.  A module consists of annotated Java source code for one or more Java classes, which define how the runtime environment should be modified. A module may also contain test code that uses the runtime APIs that will be modified.  The test code can be used to execute and debug the module in the modified as well as original runtime environments.

To create a new attack module, navigate to `File` &gt; `New` &gt; `Other...` &gt; `JReFrameworker` &gt; `JReFrameworker Runtime Project`.

<center>
![New Module](/JReFrameworker/tutorial/hidden_file_images/NewProject.png)
</center>

Enter a new project name for the module.

<center>
![Module Name](/JReFrameworker/tutorial/hidden_file_images/NewProjectName.png)
</center>

## Adding Test Logic

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

In an unmodified runtime, the print out should return "true" assuming the file could be written.  In the event that a file could not be written an exception will be thrown causing stack trace to be written to the output.

Our goal is to modify the runtime so that the print out reads "false" if the `File` object's name is "secretFile" regardless if the file actually exists on the file system, while maintaining the original functionality of the `File.exists()` method in all other cases.

## Prototyping Intended Behavior

Let's prototype a class that has the behavior we intend to modify the runtime with by developing the class as if we had control of the source code to the `File` class.  Since we are designing special case of `java.io.File` this a prime example of how Object Oriented languages can leveraged to make a subclass containing the desired behavior.

In this tutorial we will use the Eclipse New Class Wizard to create a subclass of `java.io.File`.  We create a class named `HiddenFile` that extends `java.io.File` in the package `java.io`.

<center>
![New Class Wizard](/JReFrameworker/tutorial/hidden_file_images/NewClassWizard.png)
</center>

Now because the `File` class does not have a default constructor, creating a subclass of `File` causes a compile error if we do not also create a `HiddenFile` constructor.  

<center>
![Compile Error](/JReFrameworker/tutorial/hidden_file_images/CompileError.png)
</center>

In this tutorial we use Eclipse to resolve the compile error by generating a `HiddenFile` constructor.  Optionally, we can also use Eclipse to resolve the warning that `HiddenFile` does not declare a `serialVersionUID` field.

<center>
![Successful Compile](/JReFrameworker/tutorial/hidden_file_images/SuccessfulCompile.png)
</center>

Now that we have created a subclass of `java.io.File` we can override the behavior of the `File.exists()` method with our desired functionality. First we can levearge the inherited `File.isFile()` and `File.getName()` methods to check if the `File` object is a file (and not a directory) and that the filename matches "secretFile".  If both conditions are true we can immediately return false.  Since we wish for the functionality of `HiddenFile.exists()` to behave normally in all other cases we can simply call `File.exists()` using the [super keyword](https://docs.oracle.com/javase/tutorial/java/IandI/super.html) to access the parent's method implementation.  After making these modifications we arrive at the following implementation for the `HiddenFile` class.

	package java.io;
	
	public class HiddenFile extends File {
	
		private static final long serialVersionUID = 1L;
	
		public HiddenFile(String name) {
			super(name);
		}
		
		@Override
		public boolean exists(){
			if(isFile() && getName().equals("secretFile")){
				return false;
			} else {
				return super.exists();
			}
		}
	
	}

Note that we use the source level annotation `@Override` here to ensure that the exists method is actually overriding `File.exists()`.  Source methods such as `@Override` do not get compiled into the resulting bytecode.

We can test the implementation of our prototype `HiddenFile` class by modifying our `Test` class code to instantiate a `File` object of type `HiddenFile`.  If the test logic does not produce the desired result, we can take this opportunity to set breakpoints in the `HiddenFile` class and debug it appropriately.

	import java.io.File;
	import java.io.FileWriter;
	import java.io.HiddenFile;
	import java.io.IOException;
	
	public class Test {
	
		public static void main(String[] args) throws IOException {
			File testFile = new HiddenFile("secretFile");
			FileWriter fw = new FileWriter(testFile);
			fw.write("blah");
			fw.close();
			System.out.println("Secret File Exists: " + testFile.exists());
			testFile.delete();
		}
	
	}
	
**Important Note:** The `java.io` package is a restricted package.  Running the test logic that includes a class extending a class in a restricted package will likely throw an exception depending on your current runtime security policy.

	Exception in thread "main" java.lang.SecurityException: Prohibited package name: java.io
	
If need be, this issue can be avoided by moving the `HiddenFile` class to a non-restricted package such as the default package or a new package such as `modules.java.io`.

## JReFrameworker Annotations

At this point we will take a short digression to examine the annotations provided by the JReFrameworker plugin.  There are two primary classes of annotations: define and merge annotations.  For both classes of annotations there are three levels that annotations may be applied: type, method, and field.  The following matrix defines each supported annotation type.

|            | **Define**      | **Merge**      |
|------------|-----------------|----------------|
| **Type**   | *@DefineType*   | *@MergeType*   |
| **Method** | *@DefineMethod* | *@MergeMethod* |
| **Field**  | *@DefineField*  | unsupported    |

<br />

| **Annotation Type** | **Description**                                                                                                                                                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *@DefineType*       | Inserts or overwrites the class or interface into the runtime with the fields and methods of the current type.                                                                                                                          |
| *@DefineMethod*     | Inserts or overwrites the method into the runtime type. The current type must also be annotated with *@MergeType*.                                                                                                                      |
| *@DefineField*      | Inserts or overwrites the field into the runtime type.  The current type must also be annotated with *@MergeType*.                                                                                                                      |
| *@MergeType*        | Merges the fields and methods of the current type into the existing runtime type based on the field and method annotation types.                                                                                                        |
| *@MergeMethod*      | Replaces the method in the existing runtime type.  The original runtime method is renamed and made private.  Calls using the super keyword to the original method are replaced with dynamic invocations to the renamed original method. |

## Modifying the Runtime
Coming soon...