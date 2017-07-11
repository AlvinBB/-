# css垂直居中布局总结

### line-height方法

> 该方法适用于子元素为文字的情况

```html

 #parent{
  width:400px;
  height:400px;
  text-align:center;
  line-height:400px;
 }


```

### table-cell方法

> 利用table-cell控制子行内块元素vertical-align，适用于文字和块元素

```html

#parent{
 width:400px;
 height:400px;
 background:red;
 display:table-cell;
 vertical-align: middle;
 text-align:center;
}
#child{
 width:200px;
 height:200px;
 background:green;
 display:inline-block;
}

```

### margin:auto方法

> 不适用于文字

```html

#parent{
 width:400px;
 height:400px;
 background:red;
 position:relative; 
}
#child{
 width:200px;
 height:200px;
 background:green;
 position:absolute;
 top:0;
 right:0;
 bottom:0;
 left:0;
 margin:auto;
}

```

### translate方法

> 适用于文字和块元素

```html

#parent{
 width:400px;
 height:400px;
 background:red;
 position:relative;
}
#child{
 width:200px;
 height:200px;
 background:green;
 position:absolute;
 top:50%;
 left:50%;
 transform:translate(-50%,-50%);
}


```

### flex布局

> 适用于文字和块元素

```html

#parent{
 width:400px;
 height:400px;
 background:red;
 display:flex;
 align-items:center;
 justify-content:center;
}
#child{
 width:200px;
 height:200px;
 background:green;
}


```

