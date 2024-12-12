运行项目：

npm -i


npm run dev

explain:
Src：

基类：
Commonpopover：
鼠标悬停展示内容，向分析工具发送点击记录（ga函数），参数offset位置偏移量，trigger触发方式，title标题内容，placement位置方向。


ActivationPopover，DropoutPopover，LayerNormPopover，ResidualPopover简单调用CommonPopover实现悬停显示简单介绍。


MatrixSvg：
基于D3实现矩阵可视化，传入数据，分组方式，矩阵节点形状，悬停显示节点数值，高亮某行，节点根据数值颜色渐变（-inf灰色），绘制图形。（即交互和可视化绘制）


Matrix：
具体定义MatrixSvg中参数信息title、titlePos等文本标题及位置信息，定义内部信息获取data数组行列数，判断数据准备好调用MatrixSvg进行可视化展示。（展示信息和定制化外观）

Positional Encoding Popover：
计算位置编码。
D3颜色比例尺进行现线性插值映射颜色。
CSS实现动画过渡效果
实现悬停显示vector（n），阴影变化

QKVWeightPopover，MLPweightPopover：两者代码极其相似
gsap动画库实现点击后动画效果：
获取到预先的DOM元素embeddingRows，WeightCols，weightBiasCells、outRow后。通过透明度淡入从0.1到1实现动画效果。调整持续时间。第一次为0.1后续为0.01，优化执行速度。设置动画相对位置保证展示顺序和循环结构一致。


主体思路：
对于输出矩阵根据token逐一嵌入token元素，权重列元素、偏置单元格以及输出单元格进行淡入、显示等动画设置，配合公式区域中第一行和总计行的显示隐藏切换，模拟第一行数据参与运算并得出结果的过程；随后对其余行也进行类似的逐行动画展示，呈现出整个权重乘法运算中每行数据依次参与运算的动态过程，使整个运算过程可视化且更易于理解


Article/article：
定义Softmax，Relu函数表达式，currentPlayer等变量，通用样式等，页面结构比例配置导入。


AttentionMatrix:
颜色比例尺动态获取。
点击展开以及空白区域关闭等变量，使用gsap动画时间轴，控制打开和关闭效果。
矩阵动画效果实现先矩阵整体透明度从0变1，利用strokeDashoffset可动态绘制偏移量刚开始等于总长度逐渐减小为0，线条被逐步绘制出来，stagger实现多个路径线条逐步启动，产生顺序效果。
同时矩阵内部整体元素从缩放为0的状态逐渐显现出来。
矩阵增加复制一个矩阵将初始宽度从0变成正常宽度，通过透明度改变实现掩码效果
收缩：
整体透明度以及宽度均调整为0，动画设置。


Vectorcanvas：
及时接收外部数据、颜色比例尺等更新绘制可视化内容。


Header、Headerstack：
读取模型注意力头参数，处理多头部卡片样式层数，透明度等信息

Attention：
集成Vectorcanvas、AttentionMatrix、Headerstack绘制attention虚框、多头注意力层数，QKV区域out区域等头部区域，以及功能，展开收缩，悬停。


AnimationControl:
记录动画播放进度，暂停继续重新播放等操作，按钮滑块进度等部分。


Embedding：
点击展开收缩，更新状态变量，指定持续时间和过渡效果，透明度延迟0.5秒变成1。采用Flip.from(containerState, {...})自然过渡动画的技术，记录元素初始状态（First）和目标状态（Last）。
定义内部架构组成token，Vector。边界框样式。


InputForm:
用户输入区域组件，下拉框，输入框，生成按钮，模型交互状态管理。

LinearSoftmax：
显示最后线性映射层的结果可点击展开关闭。


MLP：
不同层向量元素以及层间的操作。

Operation、OperationGroup：
activate、dropout、Layer Normalization、residual-start、residual-end组件形式，交互反馈。


ProbabilityBar：
根据最后概率可视化柱状。


SubsequentBlocks：
可视化Transformer-blocks组件：
展示交互虚框


temperature：
接收调整温度对于模型最后输出的影响。


topBar：
页面顶部导航栏


TokenVector：
token向量可视化。

sankey:
实现各部分组件的路径映射，embedding，attention，mlp，Transformer-blocks，linear-softmax，渐变函数构建渐变元素区分不同路径之间的流向。


Store/index.ts:
建立可响应状态，可更新的状态变量重新渲染组件，计算出的派生状态实时更新。
包括：动画是否在播放，后台模型是否在运行，模型是否在获取onnx，是否load完成。
初始输入的四个语句，索引进行对应，获取模型中间输出状态，预测值，用户输入的文本，是否有模块展开，导入模型名字，具体向量高度等信息，modelMeta 派生状态，根据 selectedModel获取当前模型参数



type定义需要使用到的数据类型：
flow定义流动效果类型，起止点线位置线粗细等，动画类型；
MatrixData：二维数组；
ModelMetaData：模型元信息，层数，注意力头数，模型维度，分块数量。
HighlightedToken：高亮token，索引，值类型，fix
ExpandedBlock：展开模块id
PredictionItem组成Prediction数组，rank，token，tokenid，经过tempreature前后的logit以及概率。
ModelData：
模型输出预测情况获取模型各部分输出信息。


tailwind.config.js：
定义样式主题，字体颜色等。

Python：
实现对于GPT-2模型的读取转换导出为ONNX格式模型，并分割为多块。

+page.svelte:
引入组件，模型结构各部分，数据流向，加载onnx分块模型，状态变量，pretrainedtokenizer分词器，utils中加载函数调整参数，ex0中加载示例数据，调整各部分元素布局尺寸等。



+layout.svelte:
页面结构，页面加载状态展示，顶部导航栏交互，页面尺寸
