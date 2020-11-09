![[Fenwick Tree.bmp]]
前序和以及x+=x*(-x)

lowbit
```
int lowbit(int x) {
  // 算出x二进制的从右往左出现第一个1以及这个1之后的那些0组成数的二进制对应的十进制的数
  return x & -x;
}

```
单点修改 
```
void add(int x, int k) {
  while (x <= n) {  // 不能越界
    c[x] = c[x] + k;
    x = x + lowbit(x);
  }
}

```
求和
```
int getsum(int x) {  // a[1]……a[x]的和
  int ans = 0;
  while (x >= 1) {
    ans = ans + c[x];
    x = x - lowbit(x);
  }
  return ans;
}

```