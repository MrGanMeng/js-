### requestAnimationFrame 和定时器

很长时间以来，计时器和循环间隔一直都是 js 动画的核心技术，虽然 css 变换和动画为我们提供了实现动画的简单手段，但 js 动画很多年都没有变化

然而 requestAnimationFrame 这个 api 的出现，打破了这一局面

#### 定时器动画

当显示器的刷新频率为 60Hz 时,我们人眼不会感觉卡顿。大概相当于每秒钟重绘 60 次。

因此，最平滑的动画的最佳循环间隔是 1000ms/60 次，约等于 16.7ms 一次

当我们用定时器设置 16.7ms 执行一次动画时，它真的会这样执行吗？

```jsx
const step = () => {
	animation1();
	animation2();
	// 其他动画逻辑
};

setInterval(step, 16);
```

答案是否定的，定时器只能指定把动画的逻辑添加到浏览器 UI 线程队列中等待执行的时间。如果队列前面有别的任务，那动画的逻辑就需要等别的任务完成后再执行。

简而言之，定时器中以毫秒表示的延迟时间并不代表到时间一定会执行动画逻辑。

那我们如何让浏览器按时绘制下一帧呢？

#### requestAnimationFrame

window.requestAnimationFrame() 会告诉浏览器--你希望执行一个动画，并且要求浏览器在下次重绘前调用指定的回调函数更新动画，该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘前执行。

返回值为一个 long 整数，和定时器类似，它是请求 id，是回调列表中唯一的标识。你可以传这个值给 window.cancalAnimationFrame() 以取消回调函数
