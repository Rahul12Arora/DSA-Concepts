# DSA-Concepts
DSA Concepts

<h2>DP - Dynamic Programming</h2>


```
EX-1 https://leetcode.com/problems/climbing-stairs/submissions/

//Approach 1 => TOP DOWN APPROACH
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

//Approach 2 => Tabular memoization

public int climbStairs(int n) {
        if(n<2) return n;
        int[] memo = new int[n];
        memo[0]=1;
        memo[1]=2;

        for(int i=2;i<n;i++){
            memo[i]=memo[i-1]+memo[i-2];
        }
        return memo[n-1];
    }

//APPROACH 3=> Bottom up variable

public int climbStairs(int n) {
        if(n<3) return n;
        int a = 1;
        int b = 2;

        for(int i=3;i<n+1;i++){
            int temp = b;
            b=a+b;
            a=temp;
        }
        return b;
    }
```

```
EX-2
https://leetcode.com/problems/min-cost-climbing-stairs/description/

class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        if(n==2) return Math.min(cost[0],cost[1]);
        int[] memo = new int[n+1];
        Arrays.fill(memo,-1);
       int p1 = rec(cost,0,n-1,memo);  //1
       Arrays.fill(memo,-1);
        int p2 = rec(cost,0,n-2,memo);  //0
       int m1 = Math.min(p1,p2);
       Arrays.fill(memo,-1);
       int p3 = rec(cost,1,n-1,memo);   //2
       Arrays.fill(memo,-1);
        int p4 = rec(cost,1,n-2,memo);   //1
       int m2 = Math.min(p3,p4);
        return Math.min(m1,m2);
    }
    public int rec(int[] cost, int start, int currentIndex, int []memo){

        if(currentIndex==start) return cost[start];
        if(currentIndex==start+1) return cost[start]+cost[start+1];


        if(memo[currentIndex-1]!=-1 && memo[currentIndex-2]!=-1){
            return cost[currentIndex] + memo[currentIndex-1] + memo[currentIndex-2];
        }

        if(memo[currentIndex-1]==-1){
            int p1 = rec(cost,start,currentIndex-1,memo);
            memo[currentIndex-1] = p1;
        }

        if(memo[currentIndex-2]==-1){
            int p2 = rec(cost,start,currentIndex-2,memo);
            memo[currentIndex-2] = p2;
        }


        return cost[currentIndex] + Math.min(memo[currentIndex-1],memo[currentIndex-2]);
    }
}

// 10,15,20
// 1,1,1,top => 45
// 1,2,top =>30
// 2,1,top =>35
// 2,top => 15

// 10,15,20,12,8,10
// 10 + 52
// 10 + 40

//Approach 2 = Tabulation

public int minCostClimbingStairs(int[] cost){
        int n = cost.length;
        int[] memo = new int[n+1];
        memo[0]=cost[0];
        memo[1]=cost[1];
        for(int i=2;i<n;i++)
        {
            memo[i]=cost[i]+ Math.min(memo[i-1],memo[i-2]);
        }
        return Math.min(memo[n-1], memo[n-2]);
    }

```
