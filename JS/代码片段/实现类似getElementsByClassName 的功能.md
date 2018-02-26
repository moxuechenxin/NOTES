
```js
function getByClass(el,cls){
  var start='(^|[ \n\r\t\f])'; // 匹配空白字符（空格、换行、回车、制表符、换页符）
  var end='([ \n\r\t\f]|$)';
  var reg=new RegExp(start + cls + end);
  var elList=[];
  //获取el下的所有元素
  var elements=el.getElementsByTagName("*");
  //遍历元素找出类名符合的元素
  for(var i=0,len=elements.length;i<len;i++){
    if( reg.test(elements[i].className) ){
      elList.push(elements[i]);
    }
  }
  return elList;
}
```
