```js
/* Watcher.js */
class Watcher {
  constructor(target, key, handler) {
    this.target = target
    switch (typeof key) {
      case 'string':
        this.key = key
        this.obj = target[key]
        break
      default: 
        handler = key
        this.obj = target
        break
    }
    this._obj = JSON.parse(JSON.stringify(this.obj)) // 初始值拷贝
    this.handler = handler
    this.init()
  }

  init() {
    this.deepProxy(this.target, this.key)
  }

  deepProxy (obj, key) {
    if (key) {
      const val = obj[key]
      if (typeof val === 'object') {
        this.deepProxy(val)
      }
      this.observe(obj, key)
    } else {
      Object.keys(obj).forEach(key => {
        const val = obj[key]
        if (typeof val === 'object') {
          this.deepProxy(val)
        }
        this.observe(obj, key)
      })
    }
  }

  observe = (()=> {
    let timer = null
    return function(obj, key) {
      let _value = obj[key]
      const _this = this
      Object.defineProperty(obj, key, {
        get () {
          return _value
        },
        set (val) {
          if (_value !== val && _this.handler && !timer) {
            let newVal
            if (obj === _this.target) {
              newVal = val
            } else {
              newVal = _this.obj
            }
            timer = setTimeout(() => {
              _this.handler && _this.handler.call(this, newVal, _this._obj)
              timer = null
            })
          }
          _value = val
        }
      })
    }
  })()

  unbind() {
    this.handler = null
  }
}

export default Watcher
```

```js
/* watch.js */

export default function(target, key, handler) {
  const watcher = new Watcher(target, key, handler)
  return watcher.unbind
}
```

用法：

```js
const data = {
  name: 'oigo',
  arr: [1, 2, 3]
}

const unWatch = watch(data, 'arr', (newVal, oldVal) => {
  console.log(newVal, oldVal)
})
```