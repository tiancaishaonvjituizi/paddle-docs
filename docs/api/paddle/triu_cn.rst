.. _cn_api_tensor_triu:

triu
-------------------------------

.. py:function:: paddle.triu(input, diagonal=0, name=None)


返回输入矩阵 `input` 的上三角部分，其余部分被设为0。
矩形的上三角部分被定义为对角线上和上方的元素。

参数
:::::::::
    - **input** (Tensor) : 输入Tensor input，数据类型支持 `float32`, `float64`, `int32`, `int64` 。
    - **diagonal** (int，可选) : 指定的对角线，默认值为0。如果diagonal = 0，表示主对角线; 如果diagonal是正数，表示主对角线之上的对角线; 如果diagonal是负数，表示主对角线之下的对角线。
    - **name** (str，可选)- 操作的名称(可选，默认值为None）。更多信息请参见 :ref:`api_guide_Name`。

返回
:::::::::
Tensor, 数据类型与输入 `input` 数据类型一致。

代码示例
:::::::::

.. code-block:: python

    import numpy as np
    import paddle

    data = np.arange(1, 13, dtype="int64").reshape(3,-1)
    # array([[ 1,  2,  3,  4],
    #        [ 5,  6,  7,  8],
    #        [ 9, 10, 11, 12]])


    # example 1, default diagonal
    x = paddle.to_tensor(data)
    triu1 = paddle.tensor.triu(x)
    # array([[ 1,  2,  3,  4],
    #        [ 0,  6,  7,  8],
    #        [ 0,  0, 11, 12]])

    # example 2, positive diagonal value
    triu2 = paddle.tensor.triu(x, diagonal=2)
    # array([[0, 0, 3, 4],
    #        [0, 0, 0, 8],
    #        [0, 0, 0, 0]])

    # example 3, negative diagonal value
    triu3 = paddle.tensor.triu(x, diagonal=-1)
    # array([[ 1,  2,  3,  4],
    #        [ 5,  6,  7,  8],
    #        [ 0, 10, 11, 12]])

