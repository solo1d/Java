## 目录

- [操作Zip](#操作Zip)
  - [读取zip包](#读取zip包)
  - [写入zip包](#写入zip包)

- [小结](#小结)





## 操作Zip

`ZipInputStream`是一种`FilterInputStream`，它可以直接读取zip包的内容：

```ascii
┌───────────────────┐
│    InputStream    │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│ FilterInputStream │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│InflaterInputStream│
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  ZipInputStream   │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  JarInputStream   │
└───────────────────┘
```

另一个`JarInputStream`是从`ZipInputStream`派生，它增加的主要功能是直接读取jar文件里面的`MANIFEST.MF`文件。因为本质上jar包就是zip包，只是额外附加了一些固定的描述文件。



## 读取zip包



我们来看看`ZipInputStream`的基本用法。

我们要创建一个`ZipInputStream`，通常是传入一个`FileInputStream`作为数据源，然后，循环调用`getNextEntry()`，直到返回`null`，表示zip流结束。

一个`ZipEntry`表示一个压缩文件或目录，如果是压缩文件，我们就用`read()`方法不断读取，直到返回`-1`：

```java
package myZip;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

public class myZip {
    public static void main(String[] args) throws IOException {
        // 读取 zip 包
        try (ZipInputStream readzip = new ZipInputStream(
                new FileInputStream("/Users/pp/test/gb2312.txt.zip"))
        )
        {
            ZipEntry entry = null;
            while((entry = readzip.getNextEntry()) != null)
            {
                if (!entry.isDirectory())
                {
                    int n;
                    while( (n = readzip.read()) != -1)
                    {
                        System.out.print((char)n);
                    }
                }
            }
        }
    }
}
```



## 写入zip包

`ZipOutputStream`是一种`FilterOutputStream`，它可以直接写入内容到zip包。我们要先创建一个`ZipOutputStream`，通常是包装一个`FileOutputStream`，然后，每写入一个文件前，先调用`putNextEntry()`，然后用`write()`写入`byte[]`数据，写入完毕后调用`closeEntry()`结束这个文件的打包。

```java
package myZip;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.Files;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;
import java.util.zip.ZipOutputStream;

public class myZip {
    public static void main(String[] args) throws IOException {
        // 写入ZIP包
        try(ZipOutputStream writeZip = new ZipOutputStream(
                new FileOutputStream("/Users/pp/test/java.zip"))
        )
        {
            File[] files = new File[3];  // 需要被压缩的文件
            files[0] = new File("/Users/pp/test/mkDf.sh");
            files[1] = new File("/Users/pp/test/gb2312.txt");
            files[2] = new File("/Users/pp/test/file的副本.txt");
            for (File file : files)
            {
                writeZip.putNextEntry(new ZipEntry(file.getName())); // 写入一个文件前，先调用putNextEntry()
                writeZip.write(Files.readAllBytes(file.toPath()));  // 然后用write()写入byte[]数据 到压缩包
                writeZip.closeEntry(); // 写入完毕后调用closeEntry()结束这个文件的打包
            }
        }
    }
}
```

上面的代码没有考虑文件的目录结构。如果要实现目录层次结构，`new ZipEntry(name)`传入的`name`要用相对路径。



## 小结

`ZipInputStream`可以读取zip格式的流，`ZipOutputStream`可以把多份数据写入zip包；

配合`FileInputStream`和`FileOutputStream`就可以读写zip文件。













