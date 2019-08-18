

# 创建dart web 工程

1. file -> new window, 打开一个新窗口
2. view -> Command Palette , 输入dart, 会看到提示 dart: new project, 选中它
3. 接下来就可以看到各种dart工程的模板, 其中一个是 标准web工程, 还有angular dart的
4. 然后选择目录
5. 工程就建好了

输出参考: 

```
[web-simple] pub global run stagehand web-simple
Creating web-simple application `dart_web_1`:
  C:\coding\dart\dart_web\dart_web_1\.gitignore
  C:\coding\dart\dart_web\dart_web_1\CHANGELOG.md
  C:\coding\dart\dart_web\dart_web_1\README.md
  C:\coding\dart\dart_web\dart_web_1\analysis_options.yaml
  C:\coding\dart\dart_web\dart_web_1\pubspec.yaml
  C:\coding\dart\dart_web\dart_web_1\web/favicon.ico
  C:\coding\dart\dart_web\dart_web_1\web/index.html
  C:\coding\dart\dart_web\dart_web_1\web/main.dart
  C:\coding\dart\dart_web\dart_web_1\web/styles.css
9 files written.

--> to provision required packages, run 'pub get'
exit code 0
```

接下来, 按照提示, 在terminal View输入 `pub get`, 下载依赖的包,

然后用`webdev serve`就可以启动一个http服务, 浏览器打开就可以看到界面了

回过头来, 看下建立的工程一共9个文件

首先是pubspec.yaml , 是工程的配置文件, 就像maven的pom.xml一样, 刚开始学不用关心太多, 只需要知道依赖的包在这里加就好了

然后是index.html和main.dart(就当它是js文件就好啦)

























