# jQueryをロード後に他のjsをimport

jQueryをロードしたあとに、他のjsをimportしたい場合、
($.getScript)[]を使うとよろしい。

```javascript
(function (){
  function initialize() {
    $.getScript("", function(){
      var now = moment().format();
      console.log(now);
    });
  }

  var jq = document.createElement("script");
  var jq.src = "//code.jquery.com/jquery-2.1.3.min.js";
  var jq.onload = initialize;
  document.body.appendChild(jq);
}))

```
