# 页面结构与检查片段

元素 id 每次会话都会变，定位时优先用 placeholder、class 和可见文本。

## 读题目与要求

```js
// 题干 textarea 是 disabled 的，屏幕上会截断，value 是完整的
const areas = [...document.querySelectorAll('textarea')].map(t => t.value).filter(Boolean);
const prompt = areas.sort((a, b) => b.length - a.length)[0];
```

也可直接读可访问性树里的 `题目详情与材料`、`写前指导`、`作文文体`、`最少字数`。

## 填标题与正文

```js
const titleInput = document.querySelector('input[placeholder="请输入标题"]');
const editor = document.querySelector('.composition[contenteditable="true"]');
```

粘贴文本前把段落分隔统一成单个换行：

```js
const clean = text.replace(/\n{2,}/g, "\n");
```

## 粘完自检

```js
const c = document.querySelector('.composition');
const ps = [...c.querySelectorAll('p')];
({
  chars: c.innerText.replace(/\s/g, '').length,
  paragraphs: ps.length,
  emptyParagraphs: ps.filter(p => !p.innerText.trim()).length
});
```

期望：`emptyParagraphs === 0`，`paragraphs` 约 8～10。

计数器滞后时，在正文末尾输入空格再退格：

```js
await tab.ax.pressKey(null, "ctrl+End");
await tab.ax.typeText(null, " ");
await tab.ax.pressKey(null, "BackSpace");
```

## 读分数与维度

```js
const text = document.body.innerText;
const score = (text.match(/得分\s*([\d.]+)\s*\/\s*100/) || [])[1];
const status = /批改中/.test(text) ? '批改中' : (/已提交/.test(text) ? '已提交' : '未提交');
```

维度分数在可访问性树里是 `progress indicator` 节点，按 `行文规范`、`结构严谨`、`新颖` 等文本定位相邻节点。

## 评测流程

1. 点 `智能评测`，在 `评测选项` 弹窗点 `确定`。
2. 页面跳到“我的练习”，卡片显示 `批改中`。
3. 立即点 `刷新`，分数出现。
4. 点卡片进入 `总评`、`全文批注`、`提升建议`。

## 粘正文（实测最稳）

一次合成 paste 事件塞进整篇，段落间只放单个 `\n`，平台会生成 `<div>` 段落并自动加全角缩进，不会产生空段落：

```js
const ed = document.querySelector('.composition[contenteditable="true"]');
const dt = new DataTransfer();
dt.setData('text/plain', text);
ed.dispatchEvent(new ClipboardEvent('paste', {clipboardData: dt, bubbles: true, cancelable: true}));
```

改稿时先 `ed.innerHTML = ''` 再 dispatch，避免叠加。校验：`ed.innerText` 去空白后的字符数应等于原文；`[...ed.querySelectorAll('div')].filter(d=>!d.innerText.trim()).length === 0`。

## 提交流程

1. 点 `提交`。
2. 弹窗“此操作将会提交作业到教师端，确认提交吗？”点 `确定`。
3. 确认列表卡片状态为 `已提交`（徽标变 `待批阅`），卡片右下仍显示分数。
