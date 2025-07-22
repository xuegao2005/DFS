# DFS

## 基础案例

![image-20250602203323831](/Users/xuegao/Documents/note/assets/image-20250602203323831.png)

```c++
#include <iostream>

using namespace std;

const int N =10;
int n;
int path[N];
bool st[N]; // true表示这个数被用过了

void dfs(int u){
  // 到底了
  if(u==n){
    // 输出path里存的数：123，132，213，232，312，321
		for(int i=0; i<n; i++) printf("%d ", path[i]);
    puts("");
    return; // 回溯
  }
  
  // 没到底
  for(int i=1; i<=n; i++){
    // 找到一个没有被用过的数，填到path里
    if(!st[i]){
      path[u] = i; // i这个数没有用过，填到第u层
      st[i] = true; // i现在已经被用过了
      dfs(u+1); // 继续向下填
      // 现在全部填满了，回溯的时候，恢复i的原来状态
      st[i] = false;
    }
  }
}

int main(){
  cin >> n;
  // 从0层开始，1层，2层，3层。
  dfs(0);
  
  return 0;
}

```



## n皇后

https://leetcode.cn/problems/n-queens/description/

![image-20250602210453986](../assets/image-20250602210453986.png)

法一

```c++
#include <iostream>

using namespace std;

const int N =20;

int n;
char g[N][N]; // 记录棋盘
bool col[N], dg[N], udg[N]; // 列、主对角线、副对角线

void dfs(int u){
  
  if(u == n){ // 第u行是最后一行，说明已经填满了
    for(int i = 0; i < n; i ++ ) puts(g[i]); // 输出结果 g[i](第i行的所有元素)
    puts("");
    return;
  }

  //第u行皇后应该放到哪一列，u->行，i->列
  for(int i = 0; i < n; i ++ ){ 

    // 如果第u行的第i列没有被用过，且两个对角线也没有被用过
    if(!col[i] && !dg[u + i] && !udg[u - i + n]){
      g[u][i] = 'Q'; // 在第u行的第i列放一个皇后 Q (这一行的其他位置放 . )
      col[i] = dg[u + i] = udg[u - i + n] = true; // 标记第u行的第i列被用过了
      dfs(u + 1); // 继续向下填
      // 现在全部填满了，回溯的时候，恢复原来状态
      col[i] = dg[u + i] = udg[u - i + n] = false;
      g[u][i] = '.';
    }
  }
}

int main(){
  cin >> n; // 输入棋盘的大小 n*n

  for(int i=0; i<n; i++) {
    for(int j=0; j<n; j++) {
      g[i][j] = '.'; // 初始化棋盘，'.'表示空位
    }
  }

  // 从0层开始，1层，2层，3层。
  dfs(0);
  
  return 0;
}

```

法二

```c++
#include <iostream>

using namespace std;

const int N =20;

int n;
char g[N][N]; // 记录棋盘
bool row[N], col[N], dg[N], udg[N]; // 行、列、主对角线、副对角线

void dfs(int x, int y, int s) // x->行, y->列, s->已经放置的皇后个数
{
  if(y == n) y = 0, x++; // 如果列数到达了n，换行
  if(x == n) // 最后一行时
  {
    if(s == n) //已经放了n个皇后，输出棋盘
    {
      for(int i=0; i<n; i++) puts(g[i]); // 输出棋盘
      puts("");
    }
    return;
  }

  // 不放置皇后
  dfs(x, y + 1, s);

  // 放置皇后
  if(!row[x] && !col[y] && !dg[x + y] && !udg[x - y + n]) // 行，列，对角线都没有皇后
  {
    g[x][y] = 'Q'; // 放置皇后
    row[x] = col[y] = dg[x + y] = udg[x - y + n] = true; // 标记行、列、主对角线、副对角线已被占用
    dfs(x, y + 1, s + 1); // 继续放置下一个皇后
    g[x][y] = '.'; // 回溯，撤销放置的皇后
    row[x] = col[y] = dg[x + y] = udg[x - y + n] = false; // 恢复标记
  }
}

int main()
{
  cin >> n; // 输入棋盘的大小 n*n

  for(int i=0; i<n; i++) 
  {
    for(int j=0; j<n; j++) 
    {
      g[i][j] = '.'; // 初始化棋盘，'.'表示空位
    }
  }

  dfs(0, 0, 0);
  
  return 0;
}

```

