---
title: hugo-academia主题添加代码块copy插件
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  hugo-study:
   parent: hugo
   weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## hugo-academia主题添加代码块copy插件

### 效果


1.编写`copy.css`编写好后放入`static/css`目录下
### copy.css
```css
 .highlight {
    position: relative;
}

.highlight pre {
    padding-right: 75px;
    background-color:#f8f8f8 !important;
}

.highlight-copy-btn {
    position: absolute;
    bottom: 7px;
    right: 7px;
    border: 0;
    border-radius: 4px;
    padding: 1px;
    font-size: 0.8em;
    line-height: 1.8;
    color: #fff;
    background-color: #7777
    min-width: 55px;
    text-align: center;
}

.highlight-copy-btn:hover {
    background-color: #666;
}
```



2.编写`copy.js`编写好后放入`static/js`目录下
 ### copy.js
 ```javascript
 (function() {
  'use strict';

  if(!document.queryCommandSupported('copy')) {
    return;
  }

  function flashCopyMessage(el, msg) {
    el.textContent = msg;
    setTimeout(function() {
      el.textContent = "Copy";
    }, 1000);
  }

  function selectText(node) {
    var selection = window.getSelection();
    var range = document.createRange();
    range.selectNodeContents(node);
    selection.removeAllRanges();
    selection.addRange(range);
    return selection;
  }

  function addCopyButton(containerEl) {
    var copyBtn = document.createElement("button");
    copyBtn.className = "highlight-copy-btn";
    copyBtn.textContent = "Copy";

    var codeEl = containerEl.firstElementChild;
    copyBtn.addEventListener('click', function() {
      try {
        var selection = selectText(codeEl);
        document.execCommand('copy');
        selection.removeAllRanges();

        flashCopyMessage(copyBtn, 'Copied!')
      } catch(e) {
        console && console.log(e);
        flashCopyMessage(copyBtn, 'Failed :\'(')
      }
    });

    containerEl.appendChild(copyBtn);
  }

  // Add copy button to code blocks
  var highlightBlocks = document.getElementsByClassName('highlight');
  Array.prototype.forEach.call(highlightBlocks, addCopyButton);
})();

 ```
3.修改配置文件`config.toml`

### config.toml
config.toml里添加下面这句引入`copy.css`

```toml
custom_css = ["/css/copy.css"]
```

4.引入copy.js
在主题文件夹下`/layouts/partials/site_footer.html`添加一行
### site_footer.html
```java
  <script src="/js/copy.js"></script>
```
