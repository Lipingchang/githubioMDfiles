---
title: css-drop
typora-root-url: css-drop
typora-copy-images-to: css-drop
date: 2019-08-22 15:29:43
tags:
---

### Height 

question: `height: auto` 和 `height: 100%` 的区别是啥.

```html
import { ... } from 'antd';
<Row>
	<Col>
    <div>1</div>
    <Divider style={{height: 100%}} />
    <div>2</div>
  </Col>
</Row>
```

height设置成100%之后, 他就没有高度了. 但是设置成auto, 他就占满了.

