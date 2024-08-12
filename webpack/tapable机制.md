# tapable 机制

## 一、总览

同步钩子：

- SyncHook
- SyncBailHook
- SyncWaterfallHook
- SyncLoopHook

异步并行钩子：

- AsyncParallelHook
- AsyncParallelBailHook

异步串行钩子：

- AsyncSeriesHook
- AsyncSeriesBailHook
- AsyncSeriesWaterfallHook
- AsyncSeriesWaterLoopHook

## 二、Hook 类型

- Basic Hook: 按顺序执行（类似 forEach）。
- Waterfall Hook: 前一次的结果作为后一次的输入（遇到 undefined 则跳过）
- Bail Hook: 有返回则停止执行。
- Loop Hook: 有返回则从头重新开始, 遇到 undefined 跳过。

异步并行：
- AsyncParallelHook：并行执行，通过执行 callback 结束。
- AsyncParallelBailHook：并行执行，遇到返回值则停止。

异步串行：
- AsyncSeriesHook：按顺序执行，前面执行了才会继续执行下一个。
- AsyncSeriesBailHook: 按顺序执行，有一个返回就停止。
- AsyncSeriesWaterfallHook: 按顺序执行，前一次的返回会作为后一次的参数。
- AsyncSeriesLoopHook: 按顺序执行，有返回则从头重新开始，遇到 undefined 跳过。

## 三、Tapable 拦截器

```js
hooks.intercept({
  // 每次 hook.tap() 时触发，接受tap作为参数，并支持修改 tap
  register: (tapInfo) => {},
  // hook 调用 call 时拦截
  call: (tapInfo) => {},
  // 在调用被注册的每个事件函数之前执行
  tap: (tapInfo) => {},
  // loop类型钩子中，每个函数调用前执行
  loop: (tapInfo) => {},
});
```

- register: 每次通过 `tap`、`tapAsync`、`tapPromise` 注册事件函数时触发。
- call：通过调用 `hook`实例的 `call`方法时执行。（包括 `callAsync`、`promise`）接受的参数为调用 hook 时传入的参数。
- tap：在每一个被注册的事件函数调用前执行，接受参数为对应的 tap 对象。
- loop：每次重新开始 loop 前会执行，接受的参数为调用时传入的参数。
