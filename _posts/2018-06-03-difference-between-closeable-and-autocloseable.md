---
layout: post
title: "Java-difference between closeable and autocloseable?"
date: 2018-06-03
---

Both are interfaces used to close resources. Closeable was introduced with JDK5 and now it looks as follows:

![Alt](/../images/java/closablevsautoclosable.png "closeableVSautocloseable")
~~~~
  public interface Closeable extends AutoCloseable {
  		public void close() throws IOException;
  }
~~~~

AutoCloseable was introduced with JDK7 and looks as follows:

~~~~
  public interface AutoCloseable {
  		void close() throws Exception;
  }
~~~~

Closeable has some limitations, as it can only throw IOException, and it couldn't be changed without breaking legacy code. So AutoCloseable was introduced, which can throw Exception.

AutoCloseable was specifically designed to work with try-with-resources statements. As Closeable extends AutoCloseable, you can use try-with-resources to close any resources that implement either Closeable or AutoCloseable:

~~~~
	try(FileInputStream fin = new FileInputStream(input)) {
	// Some code here
	}
~~~~

Any resource initilize with in try-with-resources(), must implement either AutoCloseable or Closeable interface.
i.e.
~~~~
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.InputStream;

public class MyResource implements AutoCloseable {

    private InputStream stream;
    public MyResource(InputStream stream){
        this.stream=stream;
    }

    @Override
    public void close() throws Exception {
        if(stream != null) {
            stream.close();
        }
    }
}
~~~~

Your implementation class should be as follows:

~~~~
public static void main(String[] args) {
    try(MyResource resource = new MyResource(new FileInputStream(new File("test.txt")))) {
        //do your stuff
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
~~~~

*Tricky part:*
We all know that every try block must have at-least a catch block or finally block. However try-with-resources may not have either a catch or finally block.
While Overriding method you may not throw Exception even though super class throws it.

~~~~
public class MyResource implements AutoCloseable {
    @Override
    public void close() {

    }
}
~~~~

Now the implementation class may not catch the excpetion.
~~~~
public static void main(String[] args) {
  try(MyResource resource = new MyResource()) {
            //do your stuff
  }
}
~~~~

Note that Closeable is idempotent, that means that calling the method close() more than once has no side effects.
