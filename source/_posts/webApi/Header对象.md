---
title: Headers 对象
description: Headers 对象
mathjax: true
tags:
  - webApi
categories:
  - webApi
abbrlink: 39
sticky: 1
swiper_index: 
date: 2023-07-07 18:19:03
updated: 2023-07-07 18:19:03
cover: https://tuchuang.voooe.cn/images/2024/05/21/image.png
---

Headers 对象
==========

网道（WangDoc.com），互联网文档计划

简介 [#](about:blank#%E7%AE%80%E4%BB%8B)
--------------------------------------

Headers 代表 HTTP 消息的数据头。

它通过`Headers()`构造方法，生成实例对象。`Request.headers`属性和`Response.headers`属性，指向的都是 Headers 实例对象。

Headers 实例对象内部，以键值对的形式保存 HTTP 消息头，可以用`for...of`循环进行便利，比如`for (const p of myHeaders)`。新建的 Headers 实例对象，内部是空的，需要用`append()`方法添加键值对。

构造函数 [#](about:blank#%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
----------------------------------------------------------

`Headers()`构造函数用来新建 Headers 实例对象。

```
const myHeaders = new Headers();

```

它可以接受一个表示 HTTP 数据头的对象，或者另一个 Headers 实例对象，作为参数。

```
const httpHeaders = {
  "Content-Type": "image/jpeg",
  "X-My-Custom-Header": "Zeke are cool",
};
const myHeaders = new Headers(httpHeaders);

```

最后，它还可以接受一个键值对数组，作为参数。

```
const headers = [
  ["Set-Cookie", "greeting=hello"],
  ["Set-Cookie", "name=world"],
];
const myHeaders = new Headers(headers);

```

实例方法 [#](about:blank#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)
----------------------------------------------------------

### append() [#](about:blank#append)

`append()`方法用来添加字段。如果字段已经存在，它会将新的值添加到原有值的末端。

它接受两个参数，第一个是字段名，第二个是字段值。它没有返回值。

```
append(name, value)

```

下面是用法示例。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "image/jpeg");

```

下面是同名字段已经存在的情况。

```
myHeaders.append("Accept-Encoding", "deflate");
myHeaders.append("Accept-Encoding", "gzip");
myHeaders.get("Accept-Encoding"); // 'deflate, gzip'

```

上面示例中，`Accept-Encoding`字段已经存在，所以`append()`会将新的值添加到原有值的末尾。

### delete() [#](about:blank#delete)

`delete()`用来删除一个键值对，参数`name`指定删除的字段名。

```
delete(name)

```

如果参数`name`不是合法的字段名，或者是不可删除的字段，上面的命令会抛错。

下面是用法示例。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "image/jpeg");
myHeaders.delete("Content-Type");

```

### entries() [#](about:blank#entries)

`entries()`方法用来遍历所有键值对，返回一个 iterator 指针，供`for...of`循环使用。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "text/xml");
myHeaders.append("Vary", "Accept-Language");

for (const pair of myHeaders.entries()) {
  console.log(`${pair[0]}: ${pair[1]}`);
}

```

### forEach() [#](about:blank#foreach)

`forEach()`方法用来遍历所有键值对，对每个指定键值对执行一个指定函数。

它的第一个参数是回调函数`callbackFn`，第二个参数`thisArg`是`callbackFn`所用的 this 对象。

```
forEach(callbackFn)
forEach(callbackFn, thisArg)

```

回调函数`callback`会接受到以下参数。

*   value：当前的字段值。
*   key：当前的字段名。
*   object：当前正在执行的 Headers 对象。

下面是用法示例。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Cookie", "This is a demo cookie");
myHeaders.append("compression", "gzip");

myHeaders.forEach((value, key) => {
  console.log(`${key} ==> ${value}`);
});

```

### get() [#](about:blank#get)

`get()`方法用于取出指定字段的字段值，它的参数就是字段名。如果字段名不合法（比如包含中文字符），它会抛错；如果字段在当前 Headers 对象不存在，它返回`null`。

```
get(name)

```

下面是用法示例。

```
myHeaders.append("Content-Type", "image/jpeg");
myHeaders.get("Content-Type"); // "image/jpeg"

```

如果当前字段有多个值，`get()`会返回所有值。

### getSetCookie() [#](about:blank#getsetcookie)

`getSetCookie()`返回一个数组，包含所有`Set-Cookie`设定的 Cookie 值。

```
const headers = new Headers({
  "Set-Cookie": "name1=value1",
});

headers.append("Set-Cookie", "name2=value2");

headers.getSetCookie();
// ["name1=value1", "name2=value2"]

```

### has() [#](about:blank#has)

`has()`返回一个布尔值，表示 Headers 对象是否包含指定字段。

```
has(name)

```

如果参数`name`不是有效的 HTTP 数据头的字段名，该方法会报错。

下面是用法示例。

```
myHeaders.append("Content-Type", "image/jpeg");
myHeaders.has("Content-Type"); // true
myHeaders.has("Accept-Encoding"); // false

```

### keys() [#](about:blank#keys)

`keys()`方法用来遍历 Headers 数据头的所有字段名。它返回的是一个 iterator 对象，供`for...of`使用。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "text/xml");
myHeaders.append("Vary", "Accept-Language");

for (const key of myHeaders.keys()) {
  console.log(key);
}

```

### set() [#](about:blank#set)

`set()`方法用来为指定字段添加字段值。如果字段不存在，就添加该字段；如果字段已存在，就用新的值替换老的值，这是它与`append()`方法的主要区别。

它的第一个参数`name`是字段名，第二个参数`value`是字段值。

```
set(name, value)

```

下面是用法示例。

```
const myHeaders = new Headers();
myHeaders.set("Accept-Encoding", "deflate");
myHeaders.set("Accept-Encoding", "gzip");
myHeaders.get("Accept-Encoding"); // 'gzip'

```

上面示例中，连续两次使用`set()`对`Accept-Encoding`赋值，第二个值会覆盖第一个值。

### values() [#](about:blank#values)

`values()`方法用来遍历 Headers 对象的字段值。它返回一个 iterator 对象，供`for...of`使用。

```
const myHeaders = new Headers();
myHeaders.append("Content-Type", "text/xml");
myHeaders.append("Vary", "Accept-Language");

for (const value of myHeaders.values()) {
  console.log(value);
}

```

  

本文转自 [https://wangdoc.com/webapi/headers](https://wangdoc.com/webapi/headers)，如有侵权，请联系删除。