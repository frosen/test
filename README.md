<br/>
<br/>
<br/>

## 全 creator 架构

<br/>
<br/>
<br/>

---

### 第一步会实现：
    一个工程，
    2 个场景，
    每个场景分几个步骤，
    其中一个步骤有交互，
    实现老师控制选场景，下一步，刷新本场景到初始化状态，师生交互

<br/>
<br/>
<br/>

---

### 分布

-   壳：数据传输和保持（恢复），显示头像（声网），播放声音（解决音频冲突），播放视频（？？？？），录音
-   creator 课堂：画线，交互，场景切换等
-   creator 课件：场景

<br/>
<br/>
<br/>

---

### 先在 H5 下实现，因为其实能不能直接下载到本地是 H5 和各个端的事情

交互数据

```javascript
data = {
    场景id: 1,
    场景数据: {
        // 服务器merge数据的话，merge到这一层
        组件1: {},
        组件2: {}
    }
};
```

两种数据：
-   变更场景(同时要删除上个场景的场景数据，保证每次进入场景都是初始化的数据状态)
-   变更场景数据

还是套用现在的模式，把学生作为服务器使用，学生不进入老师不能在一个场景内操作，也不能切换场景，这样可以避免同步问题，也对回放比较友好（可否接受）

另外，如果可以的话，希望提供一个信令用于传送事件，如果暂时不行我可以先用现在的信令模拟，
事件的特点是不记录状态，允许丢失，快就行（UDP），可以优化老师的体验

<br/>
<br/>
<br/>

---

### 还是要有场景的概念，在产品阶段需要明确

-   按照类型分：视频，交互
-   按照内容分：主画面，复习，剧情动画，教学 1，练习 1，教学 2，练习 2，复习

每个场景前会用黑屏进行过渡，表示一个内容模块的完结，内容模块之间用黑屏做停顿是为了有节奏感，我觉得我们一节课不应该一镜到底

也方便老师自行安排教学顺序，取舍内容

每个场景下，摄像头的位置最好是固定的

但在一个场景里面可以尽情发挥

<br/>
<br/>
<br/>

---

### 开发模式：

-   小叶子模式：美术主导
-   火花模式：开发主导

<br/>

步骤分成可操作和不可操作的（为了避免错误）

<br/>

双场景嵌套模式：把课堂嵌套在上层，可以实现作为 submodule 独立版本管理

场景切换结束后，每个场景自己抓一下数据（避免同步问题）
