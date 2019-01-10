## JS
js为网页添加动态效果
js的代码闭合在script标签内
### 记录一些常用的写法
- 弹出对话框
```bash
alert("content");
```
- 输出一段文字
```bash
document.write("content");
```
- 声明一个变量
```bash
var xxx;
```
- 定义一个函数
```bash
function 函数名(){
    函数体
}
```
- 可以通过点击按钮调用定义好的函数
- 有参数的函数
- 警示消息框alert
- 确认消息框confirm
- 提示消息框prompt(会弹出一个输入框)
- 获得输入框的值 
```bash
.document.getElementById('name').value;
```
## 事件
主要事件表

|事件|说明|
|----|----|
|onclick|鼠标单击事件|
|onmouseover|鼠标经过事件|
|onmouseout|鼠标移开事件|
|onchange|文本框内容改变事件|
|onselect|文本框内容被选中事件|
|onfocus|光标聚焦（eg.输入框）|
|onblur|光标离开/失焦事件|
|onload|网页导入|
|onunload|关闭网页|

## 对象
访问对象的方法
```bash
objectName.methodName()
```
- Date对象中处理时间和日期的常用方法：

|方法名称|功能描述|
|----|----|
|get/setDate()|返回/设置日期|
|get/setFullYear()|返回设置年份，用四位数表示|
|get/setYear()|返回/设置年份。0:一月；...11：十二月；所以加一|
|get/setMouth()|返回/设施月份|
|get/setHours()|返回/设置小时，二十四小时制|
|get/setMinutes()|返回/设置分钟数|
|get/setSecond()|返回/设置秒钟数|
|get/setTime()|返回/设置时间(毫秒为单位)|

   - 自定义
      - 
        ``` bash
        var d = new Date(2012,10,1);
        var d = new Date('Oct 1,2012');
        ```
- string字符串方法
```bash
.length 返回字符串长度
toUpperCase() 方法讲字符串小写字母转换为大写
```
charAt()方法可返回指定位置的字符。返回的字符长度为1的字符串
语法：
```bash
stringObject.charAt(index)
```
indexOf()方法可返回某个指定的字符串值在字符串中首次出现的
