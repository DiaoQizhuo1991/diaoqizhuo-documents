# ubuntu16.04版本runnner配置及apidemo调试说明

> 斑马HMI组  diaoqizhuo  2016-12-21

> 为达到阅读效果,请使用markdown格式查看

> atom中使用ctrl+shif+m快捷键可查看markdown格式,或者通过马克飞象web查看器进行查看

> 为了HMI的后来人更容易阅读,请阅读此文档的小伙伴,请帮助此文档修改和完善

___________


## 一. 首先是runner的配置:

1. 将钉钉中的yunos.zip文件下载并解压.

2. 添加runner环境变量: 使用vim打开home文件下的.bashrc文件,添加runner环境变量.

  1. vim .bashrc

  2. 按键盘上的 **i** 进入vim的插入模式,在.bashrc文件的最后添加alias runner='/home/diaoqizhuo/yunos/libs/precompiled/runner'


  3. 按键盘上的 **Esc** 退出vim插入模式,输入 **:wq** 保存修改
  ![](https://github.com/DiaoQizhuo1991/Markdown-photos/blob/master/runner%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE.png?raw=true)

  4. 输入cat .bashrc 查看文件是否修改成功
  ![](https://github.com/DiaoQizhuo1991/Markdown-photos/blob/master/cat%E6%9F%A5%E7%9C%8Brunner%E9%85%8D%E7%BD%AE.png?raw=true)

3. 进入/home/username/yunos目录,输入./apidemo即可执行(如果没有执行,请看下一条),按ctrl+c可关闭程序.

4. 如果运行不成功,那么证明,你跟我一样缺少libicu52库,请在钉钉下载libicu52_52.1-8ubuntu0.2_amd64.deb并安装.

  ```linux
  sudo dpkg -i libicu52_52.1-8ubuntu0.2_amd64.deb
  ```

____________

## 二. 然后是配置sublime的调试环境:

1. 第一步肯定是安装 sublime text 3 啦,这个我相信你肯定自己能想办法安装好的

2. 打开sublime text,菜单栏tools-bulid system-yunos.

3. 然后在yunos.sublime-build中添加以下代码:
  ```javascript
  {
	"cmd": ["/home/diaoqizhuo/yunos/libs/precompiled/runner","$file"]
  }
  ```
4. 然后环境就搭建成功啦,然后就可以新建一个.js文件,把下面这段代码拷贝进去ctrl+b试试吧.
  ```javascript
  "use strict";
  require("agil");

  var Window = require("caf/ui/Window");
  var View = require("caf/ui/View");
  var lang = require("caf/core/lang");
  var Page = require("caf/app/Page");
  var TextView = require("caf/ui/TextView");

  var window = new Window();
  global.Window = window;
  console.log(Window.WindowType);
  console.log("diaoqizhuo");

  var textView = new TextView();
  textView.text = "Hello World";
  textView.x = 60;
  textView.y = 80;
  textView.width = 400;
  textView.height = 180;
  textView.fontPixelSize = 60;
  textView.textColor = "#FFFFFF";
  textView.background = "#FF6600"
  textView.vAlign = TextView.VAlign.AlignVCenter;
  textView.hAlign = TextView.HAlign.AlignHCenter;
  window.addChildView(textView);

  window.show();
  ```

5. 最后,用sublime运行代码以后,关闭window不会停止运行,所以需要工具栏tools-cancel build.好啦,自己去探索新世界吧.
