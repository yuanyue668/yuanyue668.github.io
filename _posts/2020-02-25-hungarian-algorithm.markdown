---
layout: post
title:  "Hungarian Algorithm"
date:   2020-02-25 10:18:30 -0000
categories: deep_sort, mot, algorithm 
---

## What is Hungarian Algorithm?

匈牙利算法是一种能够在多项式时间内解决分配问题(assignment problem)的组合优化算法,并推动了后来的原始对偶方法。美国数学家哈罗德·库恩于1955年提出该算法。此算法之所以被称作匈牙利算法，是因为算法很大一部分是基于以前匈牙利数学家Dénes Kőnig和Jenő Egerváry的工作之上创建起来的。1957年James Munkres重新审视了这个方法，证明发现该方法是严格polynomial的，所以之后该方法也被称为Kuhn-Munkres 算法或者Munkres分配算法。原始的匈牙利算法的时间复杂度是$O(n^4)$,然而之后Edmonds和Karp，以及Tomizawa独立发现经过一定的修改，该算法能改达到$O(n^3)$的时间复杂度。 Ford和Fulkerson将该方法扩展到一般运输问题的求解上。 2006年，研究发现Carl Custav Jacobi在19实际就解决了assignment问题，并且在其逝世后的1890年求解过程被以拉丁语形式发表。

#### assignment problem

指派问题示例: 假设您正在参加一个聚会，并且您想要一个音乐家表演，一个厨师来准备食物以及一个清洁服务来帮助在聚会后打扫卫生。 有三家公司提供这三项服务中的每一项，但是一家公司一次只能提供一项服务（即B公司不能同时提供清洁工和厨师）。 您正在决定应从哪家公司购买每种服务，以最大程度地减少聚会的成本。 您意识到这是分配问题的一个示例，并着手根据以下信息制作图表：

|  Company  |   Cost for Musician  |  Cost for Chef  |   Cost for Cleaners  | 
| :-: | :-: | :-: | :-:|
| A  | $108 | $125 | $150 |
| B  | $150 | $135 | $175 |
| C  | $122 | $148 | $250 |

## Why we need Hungarian Algorithm to sovle assignment problems?

有什么方法可以解决上述分配问题？ 由于可以将上表视为3×3的系数矩阵，因此可以肯定地使用蛮力解决此问题，检查每个组合并查看产生最低价格的商品。 但是，有n！要检查组合，对于较大的n而言，此方法就会变得非常低效。

## How does the Hungarian Algorithm work?

#### Prior Knowledge
我们首先需要知道两个定理:

1. 定理1:若将分配问题的系数矩阵每一行及每一列分别减去各行及各列的最小元素，则新的分配问题与原分配问题有相同的最优解，只是最优值差一个常数。

2. 定理2:系数矩阵中的独立零元素的最多个数等于能覆盖所有零元素的最少线数
   
#### The Hungarian Method
匈牙利算法的步骤如下:

1. 将系数矩阵中的每一行减去该行中的最小数, 使该行中的最小数为0
2. 将系数矩阵中的每一列减去该列中的最小数, 使该列中的最小数为0
3. 在行和列上画线, 用尽可能少的线覆盖所有的0
4. 如果绘制了n条线(即, 等于系数矩阵的行数或列数),则此时0的分布即为最佳分配; 如果行数小于n条线, 则尚未达到最佳零数,进行下一步
5. 找到没有被任何线覆盖的最小数, 从没有被线覆盖的行中将其减去,然后将其加到被线覆盖的列中, 返回步骤3.
   
结合assignment problem中的系数矩阵,执行上述步骤, 初始系数矩阵为
$$
\left[
 \begin{matrix}
   108 & 125 & 150  \\
   150 & 135 & 175  \\
   122 & 148 & 250  \\
  \end{matrix} 
\right]
$$

1. 将系数矩阵中的每一行减去该行中的最小数, 使该行中的最小数为0, 即
$$
\left[
 \begin{matrix}
   0 & 17 & 42  \\
   15 & 0 & 40  \\
   0 & 26 & 128  \\
  \end{matrix} 
\right]
$$
2. 将系数矩阵中的每一列减去该列中的最小数, 使该列中的最小数为0
$$
\left[
 \begin{matrix}
   0 & 17 & 2 \\
   15 & 0 & 0 \\
   0 & 26 & 88 \\
  \end{matrix} 
\right]
$$
3. 在行和列上画线, 用尽可能少的线覆盖所有的0, 此时, 被删除线覆盖的列为第1列和第2行, 此时n=2 (n<3)
4. 查找没有任何行覆盖的最小数。 从没有划掉的每一行(第1, 3行)中减去该条目，然后将其添加到被划掉的每一列(第1列)中。 然后，返回到步骤3: (1) 找到未被划线的最小数为２，从第１行和第３行减去这个最小数; (2) 找到被划线的列为第1列,加上最小数目
$$
\left[
 \begin{matrix}
   -2 & 15 & 0 \\
   15 & 0 & 0 \\
   -2 & 24 & 86 \\
  \end{matrix} 
\right]
\left[
 \begin{matrix}
   0 & 15 & 0 \\
   17 & 0 & 0 \\
   0 & 24 & 86 \\
  \end{matrix} 
\right]
$$
5. 现在回到步骤3，继续画线,此时有三条线(即, 等于系数矩阵的行数或列数),最佳分配是是矩阵中0的位置,这样分配的每一行和每一列都只有一个0.
$$
\left[
 \begin{matrix}
    &  & 0 \\
    & 0 & \\
   0 & &  \\
  \end{matrix} 
\right]
$$

6. 替换成原始系数矩阵的数值,即: 150+135+122 = 407

## How to applies Hungarian Algorithm?

## Complexity Analysis

### Reference
[1] https://brilliant.org/wiki/hungarian-matching

[2] https://www.cnblogs.com/YiXiaoZhou/p/5943775.html
