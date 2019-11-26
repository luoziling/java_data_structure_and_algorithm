# Floyd算法(弗洛伊德算法)

## 简介

和Dijkstra算法一样Floyd算法也是一种用于寻找给定的加权图中顶点间最短路径的算法。该算法名称以创始人之一-1978年图灵奖获得者、斯坦福大学计算机科学系教授罗伯特·弗洛伊德命名

佛洛依德算法计算途中各个顶点之间的最短路径

的解斯特拉算法用于计算途中某一个顶点到其他顶点的最短路径。

Floyd VS Dijkstra :迪杰斯特拉算法通过选定的被访问顶点，求出从触发访问顶点到其他顶点的最短路径；弗洛伊德算法中每一个顶点都是触发访问点，所以需要将每个顶点看作被访问顶点，求出从每个顶点到其他顶点的最短路径

分析:设定点V[i]到V[k]的最短路径已知为Lik，顶点vk到vj的最短路径已知为Lkj，顶点vi到vj的最短路径为Lij则vi到vj的最短路径为min（（Lik+Lkj），Lik），vk的取值为图找那个所有顶点则可获得vi到vj的最短路径

值与vi到vk的最短路径Lik或vk到vj的最短路径Lkj，是以同样方式获得



**最小生成树与最短路径的区别**

最小生成树能够保证整个拓扑图的所有路径之和最小，但不能保证任意两点之间是最短路径。
最短路径是从一点出发，到达目的地的路径最小。





## 图解

![1573134920525](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1573134920525.png)

![1573134944028](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1573134944028.png)

![1573134969974](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1573134969974.png)

![1573134982227](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1573134982227.png)

## 代码

![1573134989402](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1573134989402.png)

```java
package priv.wzb.datastructure.algorithm.floyd;

import java.util.Arrays;

/**
 * @author Satsuki
 * @time 2019/11/6 21:25
 * @description:
 */
public class FloydAlgorithm {
    public static void main(String[] args) {
        // 测试看看图是否创建成功
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        //创建邻接矩阵
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;
        matrix[0] = new int[] { 0, 5, 7, N, N, N, 2 };
        matrix[1] = new int[] { 5, 0, N, 9, N, N, 3 };
        matrix[2] = new int[] { 7, N, 0, N, 8, N, N };
        matrix[3] = new int[] { N, 9, N, 0, N, 4, N };
        matrix[4] = new int[] { N, N, 8, N, 0, 5, 4 };
        matrix[5] = new int[] { N, N, N, 4, 5, 0, 6 };
        matrix[6] = new int[] { 2, 3, N, N, 4, 6, 0 };

        //创建 Graph 对象
        Graph graph = new Graph(vertex.length, vertex,matrix);
        //调用弗洛伊德算法
        graph.floyd();
        graph.show();
    }
}

// 创建图
class Graph{
    char[] vertex; // 存放顶点的数组
    private int[][] dis; // 从各个顶点出发到其他顶点的距离，结果也存放在这
    private int[][] pre; // 表示各个节点的前驱（要其他数组的中间节点）

    public Graph(int length,char[] vertex, int[][] dis) {

        this.vertex = vertex;
        this.dis = dis;
        this.pre = new int[length][length];
        // 初始化pre , 存放前驱顶点的下标
        for (int i = 0; i < length; i++) {
            Arrays.fill(pre[i],i);
        }

    }

    // floyd算法弗洛伊德算法
    public void floyd(){
        int len = 0; // 变量保存距离
        // 对中间顶点的遍历，k就是中间顶点的下标
        for (int k = 0; k < dis.length; k++) {
            // 从i顶点出发
            for (int i = 0; i < dis.length; i++) {
                // 从i出发通过k到达j
                for (int j = 0; j < dis.length; j++) {
                    // 求出i顶点出发经过k到达j的距离
                    len = dis[i][k] + dis[k][j];
                    // 小于直连距离
                    if (len<dis[i][j]){
                        // 更新距离
                        dis[i][j] = len;
                        // 更新前驱节点
                        pre[i][j] = k;
                    }
                }
            }
        }
    }

    // 显示pre数组和dis数组
    public void show() {

        //为了显示便于阅读，我们优化一下输出
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        for (int k = 0; k < dis.length; k++) {
            // 先将pre数组输出的一行
            for (int i = 0; i < dis.length; i++) {
                System.out.print(vertex[pre[k][i]] + " ");
            }
            System.out.println();
            // 输出dis数组的一行数据
            for (int i = 0; i < dis.length; i++) {
                System.out.print("("+vertex[k]+"到"+vertex[i]+"的最短路径是" + dis[k][i] + ") ");
            }
            System.out.println();
            System.out.println();

        }

    }
}
```

