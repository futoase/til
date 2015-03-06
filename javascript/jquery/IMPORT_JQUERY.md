# jQueryのインポート

ブックマークレットを作成しているときに
使いたい場合は以下のようにする

```javascript
(function() {

  /* jQueryをロードし終えた時に呼び出されるcall back function */
  function initialize () {
    console.log("init!");
  }

  var jq = document.createElement("script");
  var jq.src = "//code.jquery.com/jquery-2.1.3.min.js";
  var jq.onload = initialize;
  document.body.appendChild(jq);
})())
```
