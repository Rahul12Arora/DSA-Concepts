# DSA-Concepts
DSA Concepts

<ol>

<li><h2>DP - Dynamic Programming</h2></li>li>


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

//Approach 3 => Non tabular dp

public int minCostClimbingStairs(int[] cost){
int n = cost.length;
        int a =cost[0];
        int b =cost[1];
        int curr=0;
        for(int i=2;i<n;i++)
        {
            curr=cost[i]+ Math.min(a,b);
            int temp = b;
            b=curr;
            a=temp;
        }
        return Math.min(a,b);
}
```

<li><h2>Comparator & Comparable in Java</h2></li>li>

<h3>Differences between the 2 Classes</h3>

In Java, both the `Comparable` and `Comparator` interfaces are used to define the natural ordering of objects, but they serve different purposes and are used in different contexts. Here's a detailed comparison:

### Comparable Interface

**Purpose**:
- The `Comparable` interface is used to define the natural ordering of objects within the same class.

**Usage**:
- A class that implements `Comparable` must override the `compareTo` method to define the natural ordering of its instances.

**Method**:
- The `Comparable` interface has a single method: `compareTo`.

**Implementation**:
- Implementing `Comparable` involves modifying the class definition.

**Pros**:
- Simple and easy to use when natural ordering is straightforward.
- Sorting can be directly done using `Collections.sort()` or `Arrays.sort()` without passing a comparator.

**Cons**:
- Only one natural ordering can be defined within the class.
- Modifies the class itself, which might not always be desirable.

### Comparator Interface

**Purpose**:
- The `Comparator` interface is used to define multiple possible orderings of objects, allowing for custom sorting logic.

**Usage**:
- A class that defines a comparator implements the `Comparator` interface and overrides the `compare` method.

**Method**:
- The `Comparator` interface has a single method: `compare`.

**Implementation**:
- Implementing `Comparator` involves creating a separate class or using anonymous classes or lambda expressions.

**Pros**:
- Allows for multiple different orderings without modifying the class.
- More flexible and can be used for custom sorting logic.

**Cons**:
- Slightly more complex as it requires creating additional comparator classes.
- Sorting requires passing a comparator to `Collections.sort()` or `Arrays.sort()`.

### Summary

| Feature           | Comparable                        | Comparator                        |
|-------------------|-----------------------------------|-----------------------------------|
| Purpose           | Natural ordering of objects       | Custom ordering of objects        |
| Method            | `compareTo`                       | `compare`                         |
| Implementation    | Implemented in the class itself   | Implemented in separate classes   |
| Flexibility       | Single natural ordering           | Multiple custom orderings         |
| Usage Example     | `Collections.sort(list)`          | `Collections.sort(list, comparator)` |
| Pros              | Simple, easy to use               | Flexible, multiple orderings      |
| Cons              | Limited to one ordering, modifies the class | Requires additional classes, more complex |

In summary, use `Comparable` when you need a single, natural ordering for a class. Use `Comparator` when you need multiple or complex orderings or if you cannot modify the class itself.

<h3>Comparable</h3>

```
Comparable is an interface that implements compareTo method, it allows user to override this method and define one default comparing method for a class
Eg:- A Student class can implement Comparable class and use it's comparator method to sort its Student instances based on it's roll no., id or age.
We define the method while making the class and then we can use it wherever we want to

import java.io.*;
import java.util.*;

class Pair implements Comparable<Pair> {
	String x;
	int y;

	public Pair(String x, int y)
	{
		this.x = x;
		this.y = y;
	}

	public String toString()
	{
		return "(" + x + "," + y + ")";
	}

	@Override public int compareTo(Pair a)
	{
		// if the string are not equal
		if (this.x.compareTo(a.x) != 0) {
			return this.x.compareTo(a.x);        // string.compareTo => this is a method provided by string class in order to lexiographically compare 2 strings.
		}
		else {
			// we compare int values
			// if the strings are equal
			return this.y - a.y;
		}
	}
}

public class GFG {
	public static void main(String[] args)
	{

		int n = 4;
		Pair arr[] = new Pair[n];

		arr[0] = new Pair("abc", 3);
		arr[1] = new Pair("a", 4);
		arr[2] = new Pair("bc", 5);
		arr[3] = new Pair("a", 2);

		// Sorting the array
		Arrays.sort(arr);

		// printing the
		// Pair array
		print(arr);
	}

	public static void print(Pair[] arr)
	{
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
}

```

<h3>Comparator</h3>

```
class Student {
 
    // Attributes of a student
    int rollno;
    String name, address;
 
    // Constructor
    public Student(int rollno, String name, String address)
    {
 
        // This keyword refers to current instance itself
        this.rollno = rollno;
        this.name = name;
        this.address = address;
    }
 
    // Method of Student class
    // To print student details in main()
    public String toString()
    {
 
        // Returning attributes of Student
        return this.rollno + " " + this.name + " "
            + this.address;
    }
}
 
// Class 2
// Helper class implementing Comparator interface
class Sortbyroll implements Comparator<Student> {
 
    // Method
    // Sorting in ascending order of roll number
    public int compare(Student a, Student b)
    {
 
        return a.rollno - b.rollno;
    }
}
 
// Class 3
// Helper class implementing Comparator interface
class Sortbyname implements Comparator<Student> {
 
    // Method
    // Sorting in ascending order of name
    public int compare(Student a, Student b)
    {
 
        return a.name.compareTo(b.name);
    }
}
 
// Class 4
// Main class
class GFG {
 
    // Main driver method
    public static void main(String[] args)
    {
 
        // Creating an empty ArrayList of Student type
        ArrayList<Student> ar = new ArrayList<Student>();
 
        // Adding entries in above List
        // using add() method
        ar.add(new Student(111, "Mayank", "london"));
        ar.add(new Student(131, "Anshul", "nyc"));
        ar.add(new Student(121, "Solanki", "jaipur"));
        ar.add(new Student(101, "Aggarwal", "Hongkong"));
 
        // Display message on console for better readability
        System.out.println("Unsorted");
 
        // Iterating over entries to print them
        for (int i = 0; i < ar.size(); i++)
            System.out.println(ar.get(i));
 
        // Sorting student entries by roll number
        Collections.sort(ar, new Sortbyroll());
 
        // Display message on console for better readability
        System.out.println("\nSorted by rollno");
 
        // Again iterating over entries to print them
        for (int i = 0; i < ar.size(); i++)
            System.out.println(ar.get(i));
 
        // Sorting student entries by name
        Collections.sort(ar, new Sortbyname());
 
        // Display message on console for better readability
        System.out.println("\nSorted by name");
 
        // // Again iterating over entries to print them
        for (int i = 0; i < ar.size(); i++)
            System.out.println(ar.get(i));
    }
}
```

</ol>
