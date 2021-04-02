# 使用Chrome插件Scripty进行批量走单操作

原理：浏览器是一个沙盒，每次访问网址的时候都会从目标地址下载**资源**(文字，图像)和**规则**(JavaScript)并且在本地按照规则进行**拼装**并**展示**

我们做了什么：在服务器下载的JS文件中添加我们自己编写的代码，并让浏览器执行

## Step 1下载插件

打开Chrome应用商店 并下载Scripty

https://chrome.google.com/webstore/detail/scripty-javascript-inject/milkbiaeapddfnpenedfgbfdacpbcbam

![scripty_1](C:\Users\z00568298\Desktop\blog\pic\scripty_1.png)

## Step 2 添加脚本

点击Scripty图标，选择Add New Script

![scripty_2](C:\Users\z00568298\Desktop\blog\pic\scripty_2.png)

在下图界面填入名称和规则。

我们希望**网址**中**包含**https://dts-szv.clouddragon.huawei.com/DTSPortal/ticket/的时候自动执行代码

Run script if **URL Contains https://dts-szv.clouddragon.huawei.com/DTSPortal/ticket/**

记得勾选触发条件为 **Automatically On Page load** 在页面加载完成后自动触发

![scripty_3](C:\Users\z00568298\Desktop\blog\pic\scripty_3.png)

JavaScript Code取决于你想做什么事，在这里我们希望自动走单，首先需要配置问题单模板，此处略，在配置完模板之后，我们可以通过右击我们希望选中的模板名(测试模板)并点开**检查(N)**来审查元素

![scripty_4](C:\Users\z00568298\Desktop\blog\pic\scripty_4.png)

如下图

![scripty_5](C:\Users\z00568298\Desktop\blog\pic\scripty_5.png)

通过单击选中li元素（重要：确保选中的是**li元素(list item)**而不是div或者span），并右键->Copy->Copy selector，这li元素的的selector地址就被复制到了粘贴板，并把selector地址替换进document.querySelector(selector地址)

``` javascript
setTimeout(function(){
    document.querySelector("body > app-root > frameworks-root > div > div > new-dts-detail > div > div.right-container > div.tab-content.ng-star-inserted > div > div.node-template-select > d-select > div > div.devui-dropdown-menu.ng-trigger.ng-trigger-fadeInOut.ng-tns-c107-0.ng-star-inserted > ul > ul > li:nth-child(1)").click()
},3000);
setTimeout(function(){
    document.querySelector("body > app-root > frameworks-root > div > div > new-dts-detail > div > div.right-container > div.bottom-operation.ng-star-inserted > d-button:nth-child(2) > button").click()
},5000);
// 这段代码会在打开页面后3s选中模板候选框的第一个选项，再过2s点击提交按钮
// 可以直接复制进JavaScript Code
```

代码解释：`setTimeout`延迟执行函数，第一个参数是一个函数，第二个参数是延迟执行的时间，在这里我用3000毫秒，因为我尝试了On Page load立即执行，但是Chrome打开页面的逻辑并非是根据点开链接的先后而是根据用户的注意力，你会发现你当前加载的页面比先点开的页面加载得更快，而且加载的程序也并非是加载完了就能选中对应的元素，简而言之，我尝试了延迟0s，1s和2s，在打开大量的网页（~500）的时候都会失效

setTimeout的的第一个参数

```javascript
function(){
	document.querySelector("body > app-root > frameworks-root > div > div > new-dts-detail > div > div.right-container > div.tab-content.ng-star-inserted > div > div.node-template-select > d-select > div > div.devui-dropdown-menu.ng-trigger.ng-trigger-fadeInOut.ng-tns-c107-0.ng-star-inserted > ul > ul > li:nth-child(1)").click() // 选中这个地址的元素并click
}
```



第二行代码同理，右键提交并审查元素

![scripty_6](C:\Users\z00568298\Desktop\blog\pic\scripty_6.png)

右键button元素并复制selector地址

```js
function(){
    document.querySelector("body > app-root > frameworks-root > div > div > new-dts-detail > div > div.right-container > div.bottom-operation.ng-star-inserted > d-button:nth-child(2) > button").click()// 选中提交按钮并click
}
```



由于js异步，5000ms是3000ms的2000ms后，并不是5000ms后

到这里，可以尝试一下能否自动走一张单，如果可以，我们只需要一个网站就可以批量打开网址

## Step 3 批量打开网页

http://www.openurls.com.cn/

![scripty_7](C:\Users\z00568298\Desktop\blog\pic\scripty_7.png)

把网址复制到文本框中点击打开

## 注意事项

1. 这个脚本并不很智能，可以在需要走单的时候打开，平时可以进入插件管理页面关闭
2. 3s和5s只是我随便选的两个时间，一般来说时间太短有可能页面会加载不完



