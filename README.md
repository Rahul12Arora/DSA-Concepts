# DSA-Concepts
DSA Concepts

<h2>DP - Dynamic Programming</h2>


```
EX-1 https://leetcode.com/problems/climbing-stairs/submissions/

class Solution {
    public int climbStairs(int n) {
        if(n<2) return n;
        int[] memo = new int[n+1];
        Arrays.fill(memo,-1);
        memo[0]=0;
        memo[1]=1;
        memo[2]=2;
        return rec(n,memo);
    }
    public int rec(int n,int[] memo){
        if(memo[n]!=-1) return memo[n];
        memo[n]=rec(n-1,memo)+rec(n-2,memo);
        return memo[n];
    }
}
//NO NEW WAY EXPECT A DISTINCT WAY, k has only its ongoing way, so does L(because L+1 gives k which we already took in account)


// _n K+L
//  _n-1 K
//   _n-2 L
//    _3 =>2+1
//     _2=>1+1
//      _1=>1
//       _0
```
