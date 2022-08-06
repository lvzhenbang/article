# windows电脑无法链接github

解决方案如下：

1. 在C盘找到 `C:\Windows\System32\drivers\etc` 目录下的 `hosts` 文件，在文件末尾添加如下文本：

```
140.82.113.4  github.com
151.101.185.194  github.global.ssl.fastly.net
```

2. 打开命令行工具，分别输入如下命令：

```
ipconfig/displaydns  // 然后按回车键
```

```
ipconfig/flushdns  // 然后按回车键
```

3. 重新在浏览器中打开github.com进行测试
