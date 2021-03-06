# 关于Ajax中的Lazy模式

Origin X中的Ajax请求支持Lazy模式——也就是延迟加载模式。

## 1. 基本配置

```json
{
    "personal.circle.by.type": {
        "uri": "/api/:modelId/columns",
        "lazy":true
    }
}
```

仔细思考，上述URI中包含了一个路径参数`modelId`，也就是说，在页面初始化的流程中，由于`modelId`未指定，这种情况下如果发送了这个请求，会导致后端无法读取数据，并且发一次废请求，也就是说这个请求不能在页面初始化的过程中发送Ajax到后端，这种情况下，就可以启用这个Ajax的lazy模式，而在后期的DataEvent中去发送请求。

## 2. 前端交互流程

实际上前端的交互行为可以使用下边的时序图来完成，通过这个图，开发人员就理解为什么需要lazy模式了。

![](/assets/images/spo/008/lazy-mode.png)

从上图可以知道前端交互流程如下：

1. 页面初始化过程只执行一次，就是用户进入页面的时候执行。
2. 页面初始化过程中如果要发送Ajax请求，那么这个Ajax请求的参数是不依赖用户的事件触发的。
3. 一旦初始化完成过后，页面会一直等待用户输入——触发某次事件。
4. 用户触发事件过后会给Ajax请求提供参数，此时就需要发送Lazy模式的Ajax请求，并且接受回调。
5. 前端拿到回调数据过后刷新页面，继续等待输入（重复3 ~ 5）。



