# Javascript API

## 创建缘由

尽管FantasyPass已经支持了相当多的特性，基本已经满足日常所需，例如：

* 附件添加和展示
* 自动填充
* 自定义键盘
* 多种云盘、WebDAV同步支持
* 1password的支持

但是每个人用途不一，难免会需要自定义一些简单的需求，特别是开发者。例如一些简单的需求： `HTTP请求 -> 解析 -> 显示 -> 拷贝`。如果需要这么简单的操作，但是FantasyPass又并不支持这样功能，但是又不想自己写一个App。就我日常就有这样的需求，例如：定期更改自己服务的密码，这个HTTP必须保密，为了保密，我想把它放到FantasyPass里面去。但是我不能为了我一个人的需求，把这个并不通用的功能去传递给所有人。因此决定写Javascript扩展API，提供大家自定义简单功能。

## 特性

* 安全。 提供Javascript扩展，由你自己编写，自己导入，完全由自己控制。
* 权限。 设置提供三种权限选择：
    * 执行时弹窗提醒
    * 允许执行，不提示
    * 禁止Javascript Api执行
* 控制范围。 Javascript API 只能删除和修改，Javascript API增加的属性。保证主要数据的安全性。

## 安装步骤

由于Javascript都是自己写的，因此你必须把它作为附件添加到登录项里面。具体步骤如下：

* 电脑上编写Javascript脚本程序
* 把Javascript放到你的iPhone上的文件程序里面。如果是Mac，直接用AirDrop就可以发送给iPhone
* 打开FantasyPass，进入登陆项详情。点击添加附件 -> 文档 -> 选择JS文件 -> 完成添加
* 进入附件详情，点击运行即可

<img src="./IMG_4237.PNG" alt="Javascript Plugin" width="300" />

## API文档

### UI 

* $.appendRow 

    添加行

    ```
    appendRow(sectionTitle: String, sectionIndex: Int, rowType: String, key: String, value: String)
    ```

    * 参数说明：

        名称 | 说明
        --- | ---
        sectionTitle | 章节名称
        sectionIndex | 章节位置
        rowType | 行类型，目前支持： FPLabelRow
        key | 键，既是唯一标识也是现实的标题
        value | 值

    * 说明: 一个或多个Row组成Section。

## 提示框

* $.alert 

    显示Alert弹出框

    ```
    alert(title: String, message: String)
    ```

    * 参数说明：

        名称 | 说明
        --- | ---
        title | 标题
        message | 内容


* $.toast 

    显示toast提示

    ```
    toast(message: String)
    ```

    * 参数说明：

        名称 | 说明
        --- | ---
        message | 内容


* $.whisper 

    显示whisper提示框

    ```
    whisper(title: String, message: String)
    ```

    * 参数说明：

        名称 | 说明
        --- | ---
        title | 标题
        message | 内容

### HTTP请求 

* $.fetch

    发送get、post、put等请求

    ```
    fetch(url: String, params: [String : Any], completeHandler: function)
    ```

   * 参数说明：

        名称 | 说明
        --- | ---
        url | url地址
        params | 包含 method、parameters、headers字段。
        completeHandler | 完成回调, 返回字符串

    * params: 
        * method: String。 GET、HEAD、POST、PUT
        * parameters: [String: Any]。 传递的参数
        * encoding: String。 参数的编码，url、json、plist
        * headers: [String: String]。 请求头

    * 实例:
    * 
        ```
        let url = "https://raw.githubusercontent.com/kaich/FantasyPass/master/example.json"
        $.fetch(url, {}, function(responseString) {
            let json = JSON.parse(responseString)
            let code = json.results[0].imdbID
            $.appendRow("Active Code", 3, "FPLabelRow", "code", code)
        });
        ```

### 粘贴板 

* $.setPasteboard 

    复制内容到粘贴板

    ```
    setPasteboard(str: String)
    ```

   * 参数说明：

        名称 | 说明
        --- | ---
        str | 要复制的文字 


## 结语

> 初版不提供太多功能，之后有需求会陆续添加