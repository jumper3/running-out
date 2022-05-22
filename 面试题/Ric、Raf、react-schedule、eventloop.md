Ric: requestIdleCallback API
Raf: requestAnimationFrame API

如果问到这个问题，可以发散到eventloop，反之如果问到eventloop，可以诱导面试官问这里

### eventloop（浏览器）
- 首先主线程将所有js代码放入（执行栈 execution context stack），执行完所有同步任务
- 执行完毕开始检查任务队列，任务队列分为宏任务队列和微任务队列
- js同步代码可以视作一个宏任务
- 在执行一个宏任务之后，检查微任务队列是否有需要执行的任务，若有则推入执行栈，若在执行微任务的过程中遇到了微任务，则将微任务继续推入微任务队列，直到执行完所有微任务
- 微任务执行完成之后判断是否需要执行页面渲染(ui rendering)，判断的标准是是否触发了重绘、重排、距离上次渲染时间间隔是否超过了16.7ms
- 如果需要执行ui-rendering，那么在执行之前调用`RAF`回调函数，如果在这一帧里执行上述任务的时间小于16.7ms则执行`RIC`
- 如果不需要执行ui-rendering，那么判断是否还有宏/微任务（任务栈为空），如果没有了，则执行`RIC`,这个时候`RIC`的最长执行时间可以是50ms，以响应用户不可预知的操作
- 循环这个过程

### RIC
- RIC就是让js代码在浏览器空闲时间执行
- 由于他总是在eventloop的最后执行，且不一定会执行（有可能浏览器一直很忙），所以要求他所执行的函数是`不重要不紧急`的
- 如果在`RIC`中进行dom操作或者发起微任务，那么前者会导致ui-rendering重新执行，后者会导致微任务立刻执行，这样就有可能导致这一帧的执行时间超过了16.7ms，造成用户视觉上的卡顿

### react-schedule
- React16采用了fiber构架，链表式的节点可以随时暂停某个节点的渲染，将他放入下一个帧操作，避免以往的任务执行时长过长而导致的视觉卡顿（阻塞）
- 由于ric兼容性不好（ios全线不支持），外加react的渲染并不是`不重要不紧急`，且在ric中执行dom操作是不算合理的
- 所以采用了`MessageChannel + Raf`来实现，不用setTimeOut是因为后者有一个4ms的最低时延限制，且前者执行在setTimeOut之前
- 即保证了每一帧渲染完成之后，都会执行onMessage(宏任务)
- 同样，react也仿照原生eventloop写了一套（带权优先）延期队列和执行队列，将本帧无法执行的任务放入延期队列，留待下一帧执行
- 源码中叫`requestHostCallback`

### nodejs中的eventloop
- node中有两个新增的api，一个是`process.nextTick`，一个是`setImmediate`
- node中宏任务有六个队列，微任务有二个队列
- 每执行一个宏任务的阶段，都会将当前阶段对应宏任务队列里的所有任务执行（`与浏览器的最大不同`）
- 执行完一个阶段后，都会检查并执行微任务队列，优先执行nextTick的微任务队列
- 宏任务队列：`timers` 执行已经到期的定时器任务-> I/O callbacks阶段：执行一些系统调用错误，比如网络通信的错误回调 -> idle, prepare阶段：仅node内部使用 -> `poll阶段`：获取新的I/O事件，适当的条件下node将阻塞在这里 -> `check阶段`：执行setImmediate()的回调 -> colse callback阶段：执行socket的close事件回调
- timers阶段会执行已到期定时器的回调
- poll阶段会将已到期的定时器回调推入timers的任务队列中，并同步处理poll队列中的任务，直到没有任务为止
- poll队列执行完毕后，如果没有预设的setImmediate任务（check任务为空）且timers任务队列为空，就会阻塞等待在该阶段下
- check队列和timers队列任意不为空，就会继续这个循环
- check阶段执行setImmediate的回调