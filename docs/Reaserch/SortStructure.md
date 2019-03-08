# 大量结构中去除相似结构算法开发

## 背景与需求

我们通过结构表征方法将结构表征为维数一致的矩阵，通过计算矩阵之间的欧式距离来表示结构的相似程度。

现在想要开发的算法的需求是：
在大量的结构中有许多很相似的结构，通过一个算法来去除相似结构。

* 确定两个结构之间的距离小于某个阈值时将两结构视为相似结构
* 通过某种算法去除大量结构中的相似结构：

  * 现有的算法为：
        * 将第一个结构与其它结构求距离，距离小于阈值则删除（不参与之后运算）这个结构（与第一个结构求距离的另一个结构）
        * 计算第二个结构（经过第一步删除后）与剩下结构的距离，进行同样处理，之后循环这个过程到最后一个未被删除的结构
        * 这种算法能够保证结果中所有结构的距离都大于设定的阈值
        * 但我认为计算的顺序对结果有较大影响，不是很科学
        * 设置的阈值与最后筛选剩余的结果数量之间的关系不明确

  * 希望开发的新算法的特点：
        * 筛选得到的结果与计算顺序无关
        * 同样以一个阈值来区分结构是否相似