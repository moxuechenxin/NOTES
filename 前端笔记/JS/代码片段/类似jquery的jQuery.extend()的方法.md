
```js
/*** obj扩展方法(类似jQuery.extend)，返回被扩展的对象扩展之后的状态,考虑数组和对象这两种情况
 *
 * @param deep (boolean) 是否深度扩展 (可省略，默认false)
 * @param origin (object) 被扩展的对象
 * @param extra (object) 用来扩展的对象
 *
 */
function extend(){
  var agrs=arguments;
  debugger;
  var deep,origin,extra;
  var length=agrs.length;
  var start=0;
  if(typeof agrs[0] === "boolean"){
    deep=agrs[0];
    origin=agrs[1];
    start=2;
    length--;
  }else{
    deep=false;
    origin=agrs[0];
    start=1;
  }
  extra=agrs[start];
  function loop(_origin,_extra){
    //分两种情况，数组或对象
    if( typeof _origin === "object" && typeof _extra === "object" ){
      if( zj_typeof(_origin) === "object" && zj_typeof(_extra) === "object" ){
        for(var key in _extra){
          //被扩展的对象/数组对应的键值没有时或origin和extra类型不同时，origin对应key设为空对象或数组
          if( ( !_origin[key] && _origin[key]!==0 )  || zj_typeof(_origin[key]) !== zj_typeof(_extra[key]) ){
            if( zj_typeof(_extra[key]) === "array" ){
              _origin[key]=[];
            }else if( zj_typeof(_extra[key]) === "object" ){
              _origin[key]={};
            }
          }

          if( deep && typeof _origin[key] ==="object" && typeof _extra[key] ==="object" ){
            loop( _origin[key],_extra[key] );
          }else{
            _origin[key]=_extra[key];
          }
        }
      }else if( zj_typeof(_origin) === "array" && zj_typeof(_extra) === "array" ){
        var originLen=_origin.length;
        var extraLen=_extra.length;
        if(originLen>extraLen){
          _origin.splice(extraLen,originLen-extraLen);
        }
        for(var i=0,len=_extra.length;i<len;i++){
          //被扩展的对象/数组对应的键值没有时或origin和extra类型不同时，origin对应key设为空对象或数组
          if( ( !_origin[i] && _origin[i]!==0 )  || zj_typeof(_origin[i]) !== zj_typeof(_extra[i]) ){
            if( zj_typeof(_extra[i]) === "array" ){
              _origin[i]=[];
            }else if( zj_typeof(_extra[i]) === "object" ){
              _origin[i]={};
            }
          }

          if( deep && typeof _origin[i] ==="object" && typeof _extra[i] ==="object" ){
            loop( _origin[i],_extra[i] );
          }else{
            _origin[i]=_extra[i];
          }
        }
      }
    }
    //检测参数个数
     start++;
     if(start>length){
       return origin;
     }
     loop(origin,agrs[start]);
  }
  loop(origin,extra);
  return origin;
}

```
