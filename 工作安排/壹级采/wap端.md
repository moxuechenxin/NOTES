- #### wap端功能
    - [x]  **去掉欢迎界面**  
        \- main.js
    - [x]  **拍照，聊天，供货商登录跳转到下载界面**  
        \- 
    - [ ]  **微信支付改为微信公众号支付（支付成功跳转）**  
    - [x]  **详情中分享功能**  
        \- 通过url后添加search部分实现  
        \- ++main.js++  ==row:22== 修改main.html页面加载初始化加载的页面  
        \- ++productDetail.html++  ==row:778== 修改返回键切换的页面
    - [x] **订单页面点击跳转到对应栏目**  
        \- ++purchaseCenter.html++  ==row:143== changePage方法传递toItem参数，对应标签元素添加cate属性  
        \- ++ordermanagePurchase.html++  ==row:291== 修改current方法，根据传递的toItem，显示对应的栏目
    - [x] **发现页面banner图跳转问题**  
        \- ++factoryChosen.html++ ==row:247==  
        \- ++find.html++ ==row:263==
    - [x] **分享功能**  
        \- ++lcsci.html5.share.js++ ==row:2== ==row:85==   
        \- ++productDetail.html++ ==row:754==
    - [x] **微信支付**
        \- ++pay.html++ ==row:61==
    - [x] **未登录进入页面，直接切换到登陆页面**  
        \- ++main.js++  ==row:==
    - [x] **登陆成功后返回原本的页面(如分享页面)**  
        \- ++purchaserLogin.html++ ==row:65==  
        \- 
    - [x] **退出当前用户后切换到登陆页面**  
        \- ++purchaserSetting.html++  ==row:126==  
- #### 微信端bug
    - [x] **分享排版出错**  
        \- 微信flex兼容性问题，改为float布局