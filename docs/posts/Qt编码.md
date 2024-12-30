# Qt编码
##  QString::fromLocal8Bit
- 功能：将本地编码（Local8Bit，即当前系统使用的本地编码）字节数据转换为 QString。

- 适用场景：

  - 处理由本地编码表示的字符串（如 Windows 上的 GBK 或 Linux 上的 UTF-8）。
  - 从本地系统文件名、文件内容等来源读取字符串。
- 示例：
```c++
QByteArray data = "你好";
QString str = QString::fromLocal8Bit(data);
```

## QString::fromUtf8
- 功能：将 UTF-8 编码的字节数据转换为 QString。

- 适用场景：

  - 处理以 UTF-8 编码的字符串（如网络传输、JSON 文件等通常使用 UTF-8 编码的数据）。
- 示例：
```c++
QByteArray data = u8"你好";
QString str = QString::fromUtf8(data);
```
注意：如果传入的字节数据不是合法的 UTF-8 编码，可能会导致数据丢失或解析错误。

## QTextCodec::toUnicode
- 功能：将指定编码的字节数据转换为 QString。

-  适用场景：

   - 使用非默认编码的字符串数据时（如 GB2312、ISO-8859-1 等）。
   - 需要更灵活地支持多种编码场景。
- 示例：
```cpp
QTextCodec *codec = QTextCodec::codecForName("GBK");
QByteArray data = "你好"; // 假设数据是 GBK 编码
QString str = codec->toUnicode(data);
```
## QTextCodec::fromUnicode
- 功能：将 QString 转换为指定编码的字节数据。
- 适用场景：
  - 当需要将 QString 转换为非默认编码的数据（如写入文件或传输）
- 示例：
```c++
QTextCodec *codec = QTextCodec::codecForName("GBK");
QString str = "你好";
QByteArray data = codec->fromUnicode(str);
```
## 核心区别总结
|函数|输入|目标编码|灵活性|适用场景|
|--|--|--|--|--|
|QString::fromLocal8Bit|本地编码字节数据|UTF-16 (QString)|固定为本地编码|从本地编码（如 GBK）转换为 QString|
|QString::fromUtf8|UTF-8 编码字节数据|UTF-16 (QString)|固定为 UTF-8|从 UTF-8 编码的字节流转换为 QString|
|QTextCodec::toUnicode|指定编码字节数据|UTF-16 (QString)|根据指定编码（灵活）|从多种编码（如 GBK、ISO-8859-1）转换为 QString|
|QTextCodec::fromUnicode|UTF-16 (QString)|指定编码字节数据|根据指定编码（灵活）|将 QString 转换为多种编码字节数据（如 GBK、ISO-8859-1）|
## 建议的使用场景
- 如果明确知道字符串编码是 UTF-8，使用 QString::fromUtf8。
- 如果处理的是本地文件名或需要与系统本地编码兼容，使用 QString::fromLocal8Bit。
- 如果需要支持多种编码，使用 QTextCodec。
- 如果处理跨平台的字符串数据，优先考虑使用 UTF-8 作为编码格式。
## 注意事项
- QTextCodec 在 Qt 6 中已被移除，推荐使用标准库或 QString 的其他编码函数。
- 确保明确字符串的编码，避免使用错误的转换函数导致乱码或数据丢失。

## 编码解析小结
- Qt 5：QString(const char*) 按系统的本地编码解析。
- Qt 6：QString(const char*) 按 UTF-8 编码解析。

## 推荐写法：明确编码
为了避免文件编码与运行时解析编码不匹配的问题，建议始终使用显式的编码方法：

- 文件编码为 UTF-8
```cpp
const char* str = u8"HELLO中文";           // 明确指定字面量为 UTF-8
QString qStr = QString::fromUtf8(str);    // 明确按 UTF-8 解析
```
- 文件编码为 GBK
```cpp
const char* str = "HELLO中文";            // 假设文件为 GBK 编码
QString qStr = QString::fromLocal8Bit(str); // 按本地编码（如 GBK）解析
```