---
title: 去除无限debugger
date: 2022-10-29T22:55:39Z
lastmod: 2022-10-29T22:56:01Z
---

# 去除无限debugger

```js
Function.prototype.__constructor_back = Function.prototype.constructor ;
Function.prototype.constructor = function() {
  if(arguments && typeof arguments[0]==='string'){
    //alert("new function: "+ arguments[0]);
    if( "debugger" === arguments[0]){
      //arguments[0]="consoLe.Log(\"anti debugger\");";
      //arguments[0]=";";
      return
    }
  }
  return Function.prototype.__constructor_back.apply(this,arguments);
}
```

　　‍
