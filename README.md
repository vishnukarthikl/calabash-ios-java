calabash-ios-java
=================

This project implements a calabash client in Java.

Download
=========

[calabash-ios-java](https://github.com/navaneeth/calabash-ios-java/releases/)

Getting started
===============

Install Xcode and ios development tools before you setup calabash-ios-java.

It is very easy to setup calabash. Download the JAR file and add all the JAR files to the class path. Calabash-ios-java depends on Jruby and you need to add Jruby-complete also into the classpath.

```java
import calabash.java.CalabashException;
import calabash.java.CalabashRunner;

public class Program {
    public static void main(String[] args) throws CalabashException {
        // This will setup calabash and start the simulator
        CalabashRunner runner = new CalabashRunner("/path/to/your/ios/project");
        runner.setup();
        runner.start();
    }
}
```

Writting tests
==============

Finding an element and touching it

```java
import calabash.java.IOSApplication;
import calabash.java.CalabashException;
import calabash.java.CalabashRunner;
import calabash.java.UIElement;

public class Program {
    public static void main(String[] args) throws CalabashException {
        CalabashRunner runner = new CalabashRunner("/path/to/your/ios/project");
        runner.setupCalabash();
        
        IOSApplication application = runner.start();
        UIElement element = application.query("button").get(0);
        element.touch();
    }
}
```

For the query syntax, please take a look at [Calabash Wiki](https://github.com/calabash/calabash-ios/wiki/05-Query-syntax). You can use Junit asserts to perform assertions. For more information, visit [Calabash](https://github.com/calabash/calabash-ios) page.

Inspecting elements
===================

If you need to know how elements are structured in your application, you can use `inspect()` method on `Application` or `UIElement` instances. It will iterate over each element and it's child elements from which you can build a tree view.

```java
import calabash.java.IOSApplication;
import calabash.java.CalabashException;
import calabash.java.CalabashRunner;
import calabash.java.UIElement;

public class Program {
    public static void main(String[] args) throws CalabashException {
        CalabashRunner runner = new CalabashRunner("/path/to/your/ios/project");
        runner.setupCalabash();
        
        IOSApplication application = runner.start();
        application.inspect(new InspectCallback() {
			public void onEachElement(UIElement element, int nestingLevel) {
				for (int i = 0; i < nestingLevel; i++) {
					System.out.print("-");
				}
				System.out.print(element.getElementClass() + "\n");
			}
		});
    }
}
```

Screenshots
==============

`takeScreenshot()` function can be used to take the screenshot. You can also listen to screenshot events which will be called whenever a screenshot is taken. Calabash ruby client takes screenshots when there is a failure. Hooking on to this event handler will let you know when screenshots are taken. 

```java
import calabash.java.IOSApplication;
import calabash.java.CalabashException;
import calabash.java.CalabashRunner;
import calabash.java.UIElement;

public class Program {
    public static void main(String[] args) throws CalabashException {
        CalabashConfiguration config = new CalabashConfiguration();
		config.setScreenshotListener(new ScreenshotListener() {
			public void screenshotTaken(String path, String imageType, String fileName) {
				System.out.println("Path: " + path + ", image type: " + imageType + ", file name: " + fileName);
			}
		});
        CalabashRunner runner = new CalabashRunner("/path/to/your/ios/project", config);
        runner.setupCalabash();
        
        IOSApplication application = runner.start();
        
        // Wait for a false condition to simulate an error
        application.waitFor(new ICondition() {
			public boolean test() throws CalabashException {
				return false;
			}
		});
    }
}
```

Licence
==========

The MIT License (MIT)

Copyright (c) 2013 Navaneeth K.N

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

