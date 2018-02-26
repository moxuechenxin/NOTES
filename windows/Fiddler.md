## 菜单栏操作
- **窗口置顶**  
\- View -- Stay on top


## 操作
- **Replay** (快捷键R)  
\- 回放，对选中连接重新发起请求
- **Go**
- **清空回话列表 (Remove all)**  
\- Ctrl+X
- **禁止缓存**  
\- Rules -- Performance -- Disable Caching
- **打断点（请求前、响应后）**  
\- 命令行：
    - 加断点：bpu/bpafter path/to/sessions  
    - 清楚断点： bpu/bpafter  
    
\- 选择session -- 点击左下角第三格  
\- 断点处可修改请求数据或响应数据
## host配置
**步骤**：Tools -- HOSTS -- 勾选启用host配置(书写格式见左下角)  
**注意路径**

## 文件代理（响应替换）
**步骤**：打开AutoResponder -- 勾选上方选项 -- 将要被替换的文件拖到AutoResponder面板 -- 选择Rule Editor第二栏选择框，找到Find a file -- save

## 前后端接口连调
#### 伪造服务器返回数据
**步骤**：同 "文件代理"
#### 伪造请求
**步骤**：打开Composer -- 将连接拖到Composer面板