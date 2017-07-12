# javascript代码实例总结

## 持续更新

### 1. avoid if else in coding

> 实例：输入文件大小（byte）的数字类型，输出对应转换后的大小（gb，mb，kb）的string类型

```javascript

//一般思路
(function () {

 var inputNum = parseInt(promt("输入文件大小"))

 function calcSize(inputNum) {
  
  var gb = Math.pow(1024, 3),
      mb = Math.pow(1024, 2),
      kb = 1024;

  if (inputNum >= gb) {
   return ((inputNum / gb).toFixed(1) + 'gb')
  } else if (inputNum >= mb) {
   return ((inputNum / mb).toFixed(1) + 'mb')
  } else if (inputNum >= kb) {
   return ((inputNum / kb).toFixed(1) + 'kb')
  } else {
   return (inputNum.toFixed(1) + 'byte'
  }

 }

 var output = calcSize(inputNum)
 console.log(output)

})()

```

```javascript

//避免判断过多（目前只能想到这些）
(function () {

 var inputNum = parseInt(promt("输入文件大小"))

 function calcSize(inputNum) {

  var unitObj = {
   gb : [Math.pow(1024, 3), Infinity],
   mb : [Math.pow(1024, 2), Math.pow(1024, 3)],
   kb : [1024, Math.pow(1024, 2)]
  }

  //for in 循环没有先后顺序
  for (var key in unitOjb) {
   
   var unit = unitObj[key]   

   if (inputNum >= unit[0] && inputNum < unit[1]) {
    return ((inputNum / unit[0]).toFixed(1) + key)
   }

  }

  return (inputNum.toFixed(1) + 'byte')

 }
 
 var output = calcSize(inputNum)
 console.log(output)

})()

```

