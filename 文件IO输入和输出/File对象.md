## 目录

- [File对象](#File对象)
- [创建和删除文件](#创建和删除文件)
- [创建临时文件](#创建临时文件)
- [遍历文件和目录](#遍历文件和目录)
- [小结](#小结)
- 



## File对象

在计算机系统中，文件是非常重要的存储方式。Java的标准库`java.io`提供了`File`对象来操作文件和目录。

要构造一个`File`对象，需要传入文件路径。

`File`对象既可以表示文件，也可以表示目录。特别要注意的是，构造一个`File`对象，即使传入的文件或目录不存在，代码也不会出错，因为构造一个`File`对象，并不会导致任何磁盘操作。只有当我们调用`File`对象的某些方法的时候，才真正进行磁盘操作。

调用`isFile()`，判断该`File`对象是否是一个已存在的文件，调用`isDirectory()`，判断该`File`对象是否是一个已存在的目录：

```java
package myFile;

import java.io.File;
import java.io.IOException;

public class myFile {
    public static void main(String[] args) throws IOException {
        File f = new File("..");
        System.out.println(f);
        System.out.println(f.isFile());
        System.out.println(f.isDirectory());
        System.out.println(f.length());
        System.out.println(f.getPath());
        System.out.println(f.getAbsolutePath());
        System.out.println(f.getCanonicalPath());
    }
}
```

用`File`对象获取到一个文件时，还可以进一步判断文件的权限和大小：

- `boolean canRead()`：是否可读；
- `boolean canWrite()`：是否可写；
- `boolean canExecute()`：是否可执行；
- `long length()`：文件字节大小。

对目录而言，是否可执行表示能否列出它包含的文件和子目录。



## 创建和删除文件

当File对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除该文件：

```java
package myFile;

import java.io.File;
import java.io.IOException;

public class myFile {
    public static void main(String[] args) throws IOException {
        File f = new File("/Users/old/Downloads/test/newFile.txt");
        System.out.println(f);
        if (f.createNewFile())
        {
            System.out.println("文件创建成功:");
            int i = System.in.read();
            if(f.delete())
            {
                System.out.println("文件删除成功:");
            }
        }
    }
}
```



## 创建临时文件

有些时候，程序需要读写一些临时文件，File对象提供了`createTempFile()`来创建一个临时文件，以及`deleteOnExit()`在JVM退出时自动删除该文件。

```java
package myFile;

import java.io.File;
import java.io.IOException;

public class myFile {
    public static void main(String[] args) throws IOException {
        File fTemp = File.createTempFile("tmp-", ".txt");
        fTemp.deleteOnExit();
        System.out.println(fTemp.isFile());
        System.out.println(fTemp.getAbsolutePath());
    
    }
}
```



## 遍历文件和目录

当File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`提供了一系列重载方法，可以过滤不想要的文件和目录：

```java
package myFile;

import java.io.File;
import java.io.FilenameFilter;
import java.io.IOException;

public class myFile {
    public static void main(String[] args) throws IOException {
        File Flist = new File("/Users/old/Downloads");
        File[] fs1 = Flist.listFiles();  // 列出所有文件和子目录
        printFiles(fs1);
        File[] fs2 = Flist.listFiles(new FilenameFilter() {  // 仅列出.txt 文件
            @Override
            public boolean accept(File dir, String name) {
                return name.endsWith(".txt");  // 返回true表示接受该文件
            }
        });
        printFiles(fs2);

    }

    static  void printFiles(File[] files){
        System.out.println("=================");
        if( files != null)
        {
            for (File f : files)
                System.out.println(f);
        }
        System.out.println("=================");
    }

}

```

和文件操作类似，File对象如果表示一个目录，可以通过以下方法创建和删除目录：

- `boolean mkdir()`：创建当前File对象表示的目录；
- `boolean mkdirs()`：创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
- `boolean delete()`：删除当前File对象表示的目录，当前目录必须为空才能删除成功。



**如果需要对目录进行复杂的拼接、遍历等操作，使用`Path`对象更方便。**



