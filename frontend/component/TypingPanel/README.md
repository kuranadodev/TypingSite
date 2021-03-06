# TypingPanel

双拼练习的文章显示面板

## 用法

需要首先引入EventEmitter：

```js
<script src="https://cdn.bootcss.com/EventEmitter/5.2.3/EventEmitter.min.js"></script>
```

使用示例：

```js
tppanel = new TypingPanel(keyMap, zero);

// 监听typingPanel发出的事件
tppanel.on('need', info => console.log(info));
tppanel.on('finish', _ => alert('敲完了'));

// 使用新文章初始化
tppanel.init(article);

document.body.appendChild(tppanel.el);

// 监听页面上的输入事件
window.addEventListener('keypress', ev => {
    tppanel.enterKey(ev.key);
});
```

## API

### TypingPanel.constructor(keyMap, zero)

> 构造一个双拼练习面板

#### 参数

`keyMap`：键位布局，参见 XXXX

`zero`：零声母，参见 XXXX

---

### TypingPanel.prototype.init(article)

> 使用一篇文章来初始化typingPanel的内容

#### 参数

`article`：文章，参见 XXXX

#### 返回值

初始化失败返回`false`，初始化成功返回`true`。

初始化失败的原因通常为文章不符合规范：长度为0、文章以符号开头、`article`的数据格式错误

#### 注意事项

在调用该方法之前先完成对`need`事件的监听，否则会错过第一次`need`事件。

---

### TypingPanel.prototype.el

> TypingPanel实例的视图元素

---

### TypingPanel.prototype.on(eventName: String, callback: Function)

> 监听事件

---

### TypingPanel.prototype.off(eventName: String)

> 取消监听事件

---

### TypingPanel.prototype.enterKey(key: String)

> 输入一个键


#### 参数

`key`：需要输入的按键键名，可以为任意字符串，当且仅当按键为`key_map`中的键时可能返回真

#### 返回值

如果输入的按键键名为期望的按键键名则返回`true`，否则返回`false`

---

## 事件

### need

> 该事件在调用`TypingPanel.prototype.init`方法时返回`true`时触发一次，之后在每次调用`TypingPanel.prototype.enterKey`方法返回`true`时触发

该事件携带了下一个期望的键入信息：

```js
typingPanel.on('need', info => {
    info.type;  // 值为'sm'或'ym'，表示下一个需要的是声母还是韵母
    info.key;   // 值为key_map中的键，表示下一个需要的按键
    info.py;    // 值为key_map中的值，表示下一个需要的声母或韵母
})
```

---

### hit

> 该事件在调用`TypingPanel.prototype.enterKey`方法正确键入一个按键时触发

---

### miss

> 该事件在调用`TypingPanel.prototype.enterKey`方法错误键入一个按键时触发

---

### pass

> 该事件在完成一个汉字的键入时触发

该事件携带了当前文章键入完成度信息：

```js
typingPanel.on('pass', info => {
    info.total;     // 文章汉字总数
    info.current;   // 当前完成的汉字数
})
```

---

### finish

> 该事件在文章键入完成时触发