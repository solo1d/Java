## 目录

- [Path对象](#Path对象)
- [小结](#小结)



## Path对象

Java标准库还提供了一个`Path`对象，它位于`java.nio.file`包。`Path`对象和`File`对象类似，但操作更加简单：

如果需要对目录进行复杂的拼接、遍历等操作，使用`Path`对象更方便。

```java
package myFile;

import java.io.File;
import java.io.FilenameFilter;
import java.io.IOException;
import java.nio.file.Path;
import java.nio.file.Paths;

public class myFile {
    public static void main(String[] args) throws IOException {
        Path p1 = Paths.get(".", "project", "study");
        System.out.println(p1);
        Path p2 = p1.toAbsolutePath();  // 转换为绝对路径
        System.out.println(p2);
        Path p3 = p2.normalize();      // 转换为规范路径
        System.out.println(p3);
        File fPath = p3.toFile();
        System.out.println(fPath);
        for (Path p : Paths.get("..").toAbsolutePath()){
            System.out.println("  " + p );
        }
    }
}

```

