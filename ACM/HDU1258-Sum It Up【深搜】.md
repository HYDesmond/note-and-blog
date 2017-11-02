# [Sum It Up](http://acm.hdu.edu.cn/showproblem.php?pid=1258)
## 思路：
要找出后面数字和等于前面的所有组合按照从大到小的顺序，直接用深搜的话有一个问题就是会出现就是如果有重复数字就会有重复的结果，所以需要一个去重的操作，递归的同一层中如果循环到跟上一个相同的数字就跳过，这样就解决重复的问题了。
## 代码：
```cpp
#include <bits/stdc++.h>

using namespace std;
int num[1005],target[1005];
int n,m,k=1,flag=0;
int dfs(int i,int sum)
{
    if(sum == n)
    {
        for(int j=0;j<k-1;j++)
            printf("%d+",target[j]);
        printf("%d\n",target[k-1]);
        flag = 1;
    }
    if(sum < n)
    for(int j=i;j<m;j++)
    {
        if(j>i&&num[j]==num[j-1]) continue;//去重操作
        sum+=num[j];
        target[k++] = num[j];
        dfs(j+1,sum);
        sum-=num[j];
        k--;
    }
}
int main()
{

    while(~scanf("%d %d",&n,&m),n+m)
    {
        flag = 0;
        k = 0;
        memset(num,0,sizeof(num));
        memset(target,0,sizeof(num));
        for(int i = 0;i < m; i++)
        {
            scanf("%d",&num[i]);
        }
        printf("Sums of %d:\n",n);
        dfs(0,0);
        if(!flag) printf("NONE\n");
    }
    return 0;
}

```
