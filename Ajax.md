### 1. Ajax

-  Ajax = Asynchronous JavaScript and XML(异步的javaScript 和 XML)
- Ajax是一种无需要重新加载整个网页的情况下，能够更新部分网页的技术

```js
function f() {
    $.ajax({
        url: "${pageContext.request.contextPath}/ajax/getBook",
        data: {"bookName": $("#bookName").val()},
        success: function callback(data) {
            alert(data)
        },
    })
}
```