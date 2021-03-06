# io

- [理解 C10K 问题](https://www.cnblogs.com/zafu/p/8251936.html)
- [select poll epoll](https://www.cnblogs.com/Anker/p/3265058.html)

## Stream

The `java.io` package provides several output stream and input stream
classes that are descendants of its abstract `OutputStream` and `InputStream`
classes.

![stream](images/io-stream.png)

## Writer Reader

- [draw.io(writer-reader)](draw.io/writer-reader.xml)

JDK 1.0’s I/O capabilities are suitable for streaming bytes, but cannot
properly stream characters because they don’t account for character
encodings. JDK 1.1 overcame this problem by introducing writer/reader
classes that take character encodings into account. For example, the
`java.io` package includes `FileWriter` and `FileReader` classes for writing
and reading character streams.
Chapter 5 explores various writer

## Writer

![Writer](images/writer.png)

## Reader

![Reader](images/reader.png)

## 总结

java 中的`OutputStream` and `InputStream` 是面向`字节流`,但是他们不支持`character encodings`(编码/解码)
因此引入了`Writer` and `Reader`来支持编码.

- 常用的 stream 类：`FileInputStream`,`FileOutputStream`,`BufferedInputStream`,`BufferedOutputStream`,`ByteArrayInputStream`
- 常用的 reader 类：`FileReader`,`BufferedReader`
- 常用的 writer 类：`FilterWriter`,`BufferedWriter`
