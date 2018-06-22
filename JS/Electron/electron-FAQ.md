### 在渲染进程引用electron，报：`fs.existsSync is not a function`
**环境**：webpack + react
**原因**：webpack对require和import做了处理
[**参考**](https://stackoverflow.com/questions/43966353/electron-angular-fs-existssync-is-not-a-function)

**处理**：
使用`window.require()`