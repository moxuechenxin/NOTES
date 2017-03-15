- #### Deferred对象
```
    var dfd=$.Deferred();
    setTimeout(function(){
        alert(1);
        dfd.resolve();
    },1000);
    dfd.done(function(){
        alert(2);
    });

```
