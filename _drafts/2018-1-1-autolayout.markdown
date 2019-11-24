---
layout: post
title:  "Autolayout 学习笔记"
date:   2018-1-1
---

## 在 Xcode 中设置布局的三种方式：

1. 全部用代码搞定，好处是在多人协作的大型项目中，避免出现 Git 冲突；
2. XIB Files，这是 storyboard 之前的标准，缺点是  single scene，只能往里面添加 UI 元素；
3. storyboard 是当前的标准，可以添加多个 view 和 scene，但是里面的元素都是静态的，无法根据屏幕尺寸的改变自动适应，在这种情况下，Autolayout 出现了。

## Autolayout 的 4 种方式：

1. code swift 
2. visual format code
3. 在界面构建器（Interface Builder）中添加约束
4. stackview（最佳方式）

## 3 个视图属性(所有视图均具有的属性)：

1. intrinsic Content Size：intrinsic 的意思是“本质的，固有的”，顾名思义，它指的是一个 view 的固有大小，大小取决于 view 的内容。（我们不会手动改变 view 的 intrinsic Content Size 大小，除非 view 的内容改变导致其改变）
2. Compression Resistance：耐压力，Compression 指压缩，Resistance 指阻力，这个属性表示一个 view 受到压缩时候它本身的耐压性。
3. content hugging：是 Compression Resistance 的反面，当空间多大的时候，保持 View 本身不膨胀。

## stackview 的 4 个属性

1. Axis：决定 stackview 是水平排列还是垂直排列
2. Spacing：决定两个 view 之间的空间距离，优先级高于 view 本身的大小，比如空间增大的话，可能会导致 view 的大小缩小，另外，spacing 可以是负值，也就是会让两个 view 重合。
3. Alignment：决定 view 的布局模式，比如左对齐，右对齐，居中，或者顶端对齐、底部对齐或垂直居中。
4. Distribution（最重要的属性）：有两组分配算法
	- 第一组：尽量保持 view 的大小（两种算法）：第一种算法是是 equal spacing——尽量不改变每个 view 的大小，代之以使每个 view 之间的距离保持一致，如果空间不够用，就改变 Compression Resistance 最小 view ，压缩它的大小，如果所有 View 的耐压度相同，就从最左边的 view 开始压缩。第二种算法是 equal centering——跟 equal spacing 差不多，但是它测量 space 是基于 View 的中心而非边缘（在每个 view 的大小不相同的情况下，这个就不太适合了）。
	- 第二组：尽量保持 view 之间的 space，更倾向于改变 view 的大小（三种算法）：第一种是 fill（填充） equally——每个 view 占据相同的大小，如果这样做需要改变 view 的大小，它会径直这么做。第二种是 fill proportionally(成比例)——根据 View 的内容调节 view 的大小，同时可以利用 Compression Resistance 和 content hugging 这两个属性来调整某个 View 的大小，所以它是最灵活的一个。第三种是 fill（default）——跟 fill proportionally 类似，但是它会先调整第一个 view 的大小。也就是说，Fill Equally 会保证每一个其所包含的视图都适用同一种规格，而 Fill 则会总是给第一个视图尽可能大的空间

## Autolayout 的指导原则

1. 除非 stackview 解决不了问题，否则不要用约束来设置布局；
2. 从细节开始设置 stackview，然后再往整体扩展，而不是相反；
3. 和上一条类似，从内到外设置 UI，而非相反；
4. 只相信模拟器的效果，而不是看 Xcode 内的 UI 效果；
5. 不要头疼，保持耐心。

## Autolayout 的七条诫命（commandments）

1. 先挖掘 StackView 的所有属性的潜力来解决问题；
2. 如果不行，那就再嵌入一个 StackView；
3. 如果还是不行，调节 Compression Resistance 和 content hugging；
4. 如果还不行，向 StackView 添加约束；
5. 还是不行，那就像 View 添加约束；
6. 如果要连接不同的 StackView 中的 view，参考第五条；
7. 如果都不行，那就尝试 filler view。

再加一条：如果都不行，尝试 DSL，可以先试试 SnapKit。