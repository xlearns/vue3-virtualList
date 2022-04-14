# 虚拟列表
- 虚拟列表的实现，实际上就是在首屏加载的时候，只加载可视区域内需要的列表项，当滚动发生时，动态通过计算获得可视区域内的列表项，并将非可视区域内存在的列表项删除。
- 可视区域的起始索引
- 可视区域的结束索引
- 可视区域的数据
- 计算起始索引对应的数据在整个列表中偏移位置，并设置到列表上 
  - 类似一张非常大的雪碧图。可以通过偏移去获取自己想要展示的数据

## html结构
```html
<!-- 可视化容器 -->
<div class="infinite-list-container">
  <!-- 占位符。高度为总列表的高度 用于形成滚动条 -->
    <div class="infinite-list-phantom"></div>
    <!-- 渲染区域 -->
    <div class="infinite-list">
      <!-- item-1 -->
      <!-- item-2 -->
      <!-- ...... -->
      <!-- item-n -->
    </div>
</div>

```
# 滚动
- 可视区域的列数 = 容器大小 / 每一列高度
- 列表总高度 = 总共数据量 * 每一列高度)
## 更新start索引
- 索引 = `Math.floor(scrollTop / 每一列高度)`
## 更新end索引
- 索引 = start索引 + 可视区域的列数

## 更新滚动条位置
- 当滚动后，由于渲染区域相对于可视区域已经发生了偏移，此时我需要获取一个偏移量，通过样式控制将渲染区域偏移至可视区域中。
- 偏移量 = `scrollTop - (scrollTop % 每一列高度)`;
- 修改`translate3d(0,偏移量,0)`




# [element-plus  虚拟化选择器分析](https://github.com/element-plus/element-plus/blob/dev/packages/components/select-v2/src/select.vue)

# [element-plus  虚拟化Tree分析](https://element-plus.org/zh-CN/component/tree-select.html)