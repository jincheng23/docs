.. _cn_api_paddle_unsqueeze:

unsqueeze
-------------------------------

.. py:function:: paddle.unsqueeze(x, axis, name=None)

向输入 Tensor 的 Shape 中一个或多个位置（axis）插入尺寸为 1 的维度。

请注意，在动态图模式下，输出 Tensor 将与输入 Tensor 共享数据，并且没有 Tensor 数据拷贝的过程。
如果不希望输入与输出共享数据，请使用 `Tensor.clone`，例如 `unsqueeze_clone_x = x.unsqueeze(-1).clone()` 。

下图展示了一个 Shape 为 [2, 3] 的 Tensor 如何使用 ``unsqueeze`` 方法在最后一个位置（axis = 2）增加一个维度，从二维变成三维。

.. image:: ../../images/api_legend/unsqueeze.png
    :width: 700
    :alt: 图例

额外的，当需要在多个位置增加维度时， ``unsqueeze`` 方法会按顺序依次增加维度。例如 Shape 为 [2, 3] 的 Tensor 在 axis = [0, 2] 处增加维度， ``unsqueeze`` 方法会先在 axis = 0 前插入一个维度， Shape 变为 [1, 2, 3]，接着在 axis = 2 前插入一个维度， Shape 最终变为 [1, 2, 1, 3]。

参数
:::::::::
        - **x** (Tensor) - 输入的 `Tensor`，数据类型为：bfloat16、float32、float64、bool、int8、int32、int64。
        - **axis** (int|list|tuple|Tensor) - 表示要插入维度的位置。数据类型是 int32。如果 axis 的类型是 list 或 tuple，它的元素可以是整数或者形状为[]的 0-D Tensor。如果 axis 的类型是 Tensor，则是 1-D Tensor。如果 axis 是负数，则 axis=axis+ndim(x)+1 。
        - **name** (str，可选) - 具体用法请参见 :ref:`api_guide_Name`，一般无需设置，默认值为 None。

返回
:::::::::
Tensor，扩展维度后的多维 Tensor，数据类型与输入 Tensor 一致。

代码示例
:::::::::

COPY-FROM: paddle.unsqueeze
