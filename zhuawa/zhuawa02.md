1. 事件捕获是早期网景公司的浏览器的事件处理机制

2. 事件冒泡是 IE8 之前的事件处理机制

3. 标准将这两种处理方式整合在 addEventListener 中，由第三个参数决定使用哪种事件处理机制，默认为冒泡 false

   1. target.addEventLister(type, handler, useCapature = false)

   2. target.addEventLister(type, handler, options = {

      ​	once.
      ​	pastive.
      ​	useCapature 

      })

4. 捕获的使用场景

   1. 控制子元素的事件响应
   2. onError 事件

5. removeEventListener(\_, handler, true) 无法移除 addEventListener(\_, handler, false) 绑定的事件回调
   相对的，第三个参数使用 options 绑定的事件回调，要求卸载时 options 的三个属性必须一致

6. currentTarget，curentTarget 在事件处理过程中是变化的，意为当前事件处理函数执行时的指向
   target 是实际触发事件的对象，在事件处理过程中是变化的
   比如在 div>a 中，给 div 绑定事件，然后点击 a，然后调用div绑定的事件回调时，target 就是 a，curentTartget 就是 div
   常见使用场景就是在于事件委托的时候，绑定在 document 上的事件委托回调的 currentTartget 永远是 document，只有 target 才可以取到具体的元素

7. onClick 绑定事件回调是 DOM 0，只能绑定一个事件
   addEventListener 是 DOM 2 ，可以绑定多个事件（现在已经是 DOM 3）

8. 不是所有的事件都会冒泡，比如 mouseEnter，mouseEnter 就是为了 mouseOver 冒泡导致事件回调被多次执行
   比如 div>a，mouseOver 在 进入 div 时会触发一次，然后进入 a 时会因为冒泡再触发一次 div 的 mouseOver，需使用 stopProgration 阻止冒泡，而使用 mouseEnter 就不会有这个问题

9. stopProgration 会阻止捕获和冒泡的事件传递，而 IE8 之前的 cancelBuble = true 只会阻止冒泡（IE8之前 也只有冒泡）

10. stopImmediatePropagastion 还会阻止元素后续添加的事件回调，而 stopPropagation 不会
    t.addEeventListener(a, ()=>{window.stopImmediatePropagastion(); console.log('b 不会被执行') })
    t.addEeventListener(a, b)

11. 就算没有给元素绑定事件回调，事件本身也是存在的，只是没有回调去执行

12. preventDefault() 阻止元素默认行为

# 触发事件

```
const evt = new MouseEvent('click')
target.dispatch(evt)
```

# 自定义事件

```
const evt = new Event('other')
// CustomEvent 可以添加自定义属性
// const evt = new CustomEvent('other', {detail: +new Date()})
target.addEeventListener('other', ....)
target.dispatch(evt)
```

# CSS3 

1. pointer-events:none 不会在事件传递过程中被访问，常见就是遮罩透传
2. touch-action 移动端，和 pointer-events:none 类似

# XHR

1. xhr.abort()
2. xhr.timout = 1000
   xhr.ontimeout = function(){}

# 跨域



