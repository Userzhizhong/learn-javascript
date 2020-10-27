# async in JS
## concept
异步：多个相关事物的发生无需等待前一个事物的完成。一个任务可以与先前一个（或多个）的任务一起执行。而无需为了等待他们的完成而停止执行。
## JavaScript中的异步
JavaScript是单线程（主线程）执行的。
### 异步回调（asynchronous callback）
将函数作为参数传递给那些在后台中执行的其他函数。当后台中的函数执行完毕后，会调用callback函数。
e.g. 
 ``` 
  <script>
  var btn = document.getElementById('btn')
  btn.addEventListener('click',()=>{
      alert('callback works now!');
  },false);
</script>
<div id="btn">
</div>
```
这是javaScript中典型的回调函数，()=>{alert('callback works now!')}作为addEventListener的参数,当按钮被点击的时候，就会触发回调函数。
### promise

### setTimeout(),setInterVal(),requestAnimationFrame()

### async function 和 await