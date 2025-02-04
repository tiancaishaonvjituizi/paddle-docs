.. _cn_api_fluid_layers_auc:

auc
-------------------------------

.. py:function:: paddle.static.auc(input, label, curve='ROC', num_thresholds=200, topk=1, slide_steps=1)




**Area Under the Curve(AUC) Layer**

该层根据前向输出和标签计算AUC，在二分类(binary classification)估计中广泛使用。

注：如果输入标注包含一种值，只有0或1两种情况，数据类型则强制转换成布尔值。

相关定义可以在这里找到: https://en.wikipedia.org/wiki/Receiver_operating_characteristic#Area_under_the_curve 

有两种可能的曲线：

1. ROC:受试者工作特征曲线

2. PR:准确率召回率曲线

参数
::::::::::::

    - **input** (Tensor|LoDTensor) - 数据类型为float32、float64。浮点二维变量，值的范围为[0,1]。每一行降序排列。该输入为网络预测值的输入。
    - **label** (Tensor|LoDTensor) - 数据类型为int32、int64。二维整型变量，为训练数据的标签。
    - **curve** (str) - 曲线类型，可以为 ``ROC`` 或 ``PR``，默认 ``ROC``。
    - **num_thresholds** (int) - 将roc曲线离散化时使用的临界值数。默认200。
    - **topk** (int) -  取topk的输出值用于计算。
    - **slide_steps** (int) - 当计算batch auc时，不仅用当前步也用于先前步。slide_steps=1，表示用当前步；slide_steps = 3表示用当前步和前两步；slide_steps = 0，则用所有步。

返回
::::::::::::
tuple，当前计算出的AUC。数据类型是tensor，支持float32和float64。

返回的元组为auc_out, batch_auc_out, [batch_stat_pos, batch_stat_neg, stat_pos, stat_neg]。

- auc_out为准确率的结果；
- batch_auc_out为batch准确率的结果；
- batch_stat_pos为batch计算时label=1的统计值；
- batch_stat_neg为batch计算时label=0的统计值；
- stat_pos计算时label=1的统计值；
- stat_neg为计算时label=0的统计值。

代码示例
::::::::::::

.. code-block:: python

    import numpy as np

    import paddle
    import paddle.static as static
    import paddle.nn.functional as F

    paddle.enable_static()
    data = static.data(name="input", shape=[-1, 32,32], dtype="float32")
    label = static.data(name="label", shape=[-1], dtype="int")
    fc_out = static.nn.fc(x=data, size=2)
    predict = F.softmax(x=fc_out)
    result = static.auc(input=predict, label=label)

    place = paddle.CPUPlace()
    exe = static.Executor(place)

    exe.run(static.default_startup_program())
    x = np.random.rand(3,32,32).astype("float32")
    y = np.array([1,0,1])
    output= exe.run(feed={"input": x,"label": y},
                fetch_list=[result[0]])
    print(output)
    #[array([0.])]