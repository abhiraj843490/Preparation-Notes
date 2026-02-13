https://www.ambitionbox.com/interviews/accolite-interview-questions/java-developer

Horizontal scaling and vertical scaling

- Q1. What are immutable classes
- Ans. Immutable classes are those whose instances cannot be modified after creation, ensuring thread safety and predictable behavior.
    
    - 1. Immutable classes cannot be changed once created. Example: String class in Java.
        
    - 2. They help in maintaining thread safety as their state cannot be altered.
        
    - 3. To create an immutable class, declare the class as final and all fields as private and final.
        
    - 4. Provide no setters and only getters for accessing field values.
        
    - 5. Use a constructor to initialize all fields at the time of object creation.

1. what is record class how will handle the getter setter?
2. String class is public or private if public then how it is immutable?
3. how to docker image?
4. what is config map in kubernates?
5. java 8 stream problem and features
6. completible future 
7. functional programing:
8. Bean
9. primary and qualifier annotation
10. how to create immutable class?
11. Radis implementation and how is it work


12. suppose one method is throwing 2 type of error then while handling it which one go 1st to handle the exception and why?

13. tight coupling
14. agile methodology 
15. authentication vs authorization
16. What is the difference between `Collection` and `Collections` in Java?

17. what is seald class

18. vector vs array
19. set vs map
20. arraylist vs linkedlist
21. Explain how to find the fourth highest salary from a list using Java 8 streams.list.stream().filter



22. @component vs @service
23. union vs all union
24. select query for to count the all duplicate mails
		select count(mail), mail from user group by mail having count(mail)>1
		
25. hibernate cascading:  when you perform an operation on a parent entity, the same action is automatically applied to its related child entities.
26. 3 states of jwt token: header, payload and signature
27. how will know tempered token :ans signature
28. interceptor vs filter class
29. Hipa 
30. constructor chaning
31. kafka failure case how it will manage
32. equals() and hashcode()
33. what will happen when equals() override but not hashcode() and vise versa?
34. its a good idea use key in map as immutable and why
35. what is object level lock and class level with thread
36. immutable class steps and what are benefits
37. LRU cache



```java
// Find the length of the longest consecutive sequence of integers in the array.  
// int[] arr ={100, 102, 101, 200, 3, 1, 2, 4, 5, 105, 106, 104, 103};  
// output - 7  
package com.assembling;  
import java.util.HashSet;  
import java.util.Set;  
  
public class Test2 {  
    public static void main(String[]args){  

    int[] arr ={100, 102, 101, 200, 3, 1, 2, 4, 5, 105, 106, 104, 103, 98, 107};  
  
        int res = findLongcosecutive(arr);  
        System.out.println(res);  
    }  
  
    static int findLongcosecutive(int []arr){  
        int n = arr.length;  
        Set<Integer> set = new HashSet<>();  
        for(int i=0;i<n;i++){  
            set.add(arr[i]);  
        }  
        int len =0;  
        for(int val:set){  
            if(!set.contains(val-1)){  
                int curr =val;  
                int s = 1;  
                while(set.contains(curr+1)){  
                    curr++; s++;  
                }  
                len = Math.max(len, s);  
            }  
        }  
        return len;  
    }   
}
```




class A {
	A(){
		int  val =10;

	}
	A(int val){
		System.out.print(val);
	}

}



```java 
package com.assembling;  
  
import java.util.*;  
  
public class Test2 {  
  
    public static  void main(String[]args){  
        String st = "TatvaSoft is a nice company";  
        String res = sortBasedFreq(st);  
  
        System.out.println(res);  
        System.out.println(res.length());  
    }  
  
     static String sortBasedFreq(String st){  
         Map<Character, Integer> mp = new HashMap<>();  
         for (char ch: st.toCharArray()){  
             mp.put(ch, mp.getOrDefault(ch,0)+1);  
         }  
  
         List<Map.Entry<Character, Integer>> list = new ArrayList<>(mp.entrySet());  
  
         for(int i=0;i<list.size();i++){  
             for(int j=i+1;j< list.size();j++){  
                 if(list.get(i).getValue()<list.get(i).getValue()){  
                     Map.Entry<Character, Integer> temp = list.get(i);  
                     list.set(i, list.get(j));  
                     list.set(j, temp);  
                 }  
             }  
         }  
         list.sort(new Comparator<Map.Entry<Character, Integer>>() {  
             @Override  
             public int compare(Map.Entry<Character, Integer> o1, Map.Entry<Character, Integer> o2) {  
                 return o2.getValue() - o1.getValue();  
             }  
         });  
  
         StringBuilder sb = new StringBuilder();  
         for(Map.Entry<Character, Integer> e:list){  
             sb.append(e.getKey());  
         }  
        return sb.toString();  
     }    
}
```

```java
List<Integer> numbers = Arrays.asList(10, 20, 30, 40, 50);
int secondLargest = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .limit(2)
        .skip(1)
        .findFirst()
        .orElseThrow();

System.out.println(secondLargest); // 40

```


# **String**
```java
1. String str = "dcba";

String sorted = str.chars()
        .sorted()          
        .mapToObj(c -> String.valueOf((char) c))
        .collect(Collectors.joining());

System.out.println(sorted); // abcd



2. String sentence = "java is fun to learn";

String sorted = Arrays.stream(sentence.split("\\s+"))
        .sorted()                                    
        .collect(Collectors.joining(" "));           

System.out.println(sorted); // fun is java learn to



```public static  void main(String[]args){  
    HashMap<Integer, Integer> mp = new HashMap<>();  
  
    int []arr = {12, 12,45, 45, 76,89,76};  
    for(int num:arr){  
        mp.put(num, mp.getOrDefault(num,0)+1);  
    }  
  
    if(mp.containsKey(12)){  
        System.out.println("true");  
    }  
    Set<Integer> set = mp.keySet();  
    System.out.println("key set "+set.toString());  
  
    Collection<Integer> s = mp.values();  
    System.out.println("value "+s.toString());  
    List<Integer>list = new ArrayList<>();  
    for(Map.Entry<Integer, Integer> entry : mp.entrySet()){  
        if(entry.getValue()>1){  
            list.add(entry.getKey());  
        }  
    }  
  
    mp.forEach((key, val)-> mp.keySet());  
  
    System.out.println("final "+list.toString());  
}
```


// Valid Parenthesis

```java
private static boolean isValid(String s) {  
    Stack<Character> st = new Stack<>();  
    for (char ch : s.toCharArray()) {  
        if (ch == '(' || ch == '{' || ch == '[') {  
            st.push(ch);  
        } else {  
            if (st.isEmpty()) {  
                return false;  
            }  
            char top = st.peek();  
            if ((ch == ')' && top == '(') || (ch == '}' && top == '{') || (ch == ']' && top == '[')) {  
                st.pop();  
            }  
        }  
    }  
    return st.isEmpty();  
}  
  
public static void main(String[] args) {  
    assert isValid("{}");  
    assert !isValid("}{");  
    assert isValid("{}()[]");  
    assert !isValid("{()}[]}}");  
    assert !isValid("}[](){");  
    assert isValid("{[]()}");  
    assert !isValid("{([][])}{}}}");  
    assert !isValid("}}}}}}{{");  
    assert !isValid("}}[](){");  
    System.out.println("Assertion test succesful!");  
}
```



/**
 * We have a {@link Producer} thread generating some data (simulated by incrementing an integer).
 * We also have a {@link Consumer} thread which receives this data and consumes it (for this example it just prints it).
 * Both are connected via a queue.
 **/

```java
public static class Producer extends Thread {  
    // Concurrent queue: All operations on it are thread safe  
    // The choice of queue implementation is not the source of the race condition    private final ConcurrentLinkedQueue<Integer> mQueue;  
    private final Object mNotifier;  
      
    public Producer(ConcurrentLinkedQueue<Integer> queue, Object notifier) {  
        mQueue = queue;  
        mNotifier = notifier;  
    }  
  
    @Override  
    public void run() {  
        int i = 0;  
        while (true) {  
            sendOne(i);  
            i++;  
            try {  
                Thread.sleep(1); // let the consumer catch up  
            } catch (InterruptedException e) {  
                return;  
            }  
        }  
    }  
      
    /**  
     * Somewhere in this function or {@link Consumer#handleOne()} there is at least one race condition.  
     */    private void sendOne(int i) {  
    // if (mQueue.size() == 1) { // was empty before add  
        synchronized (mNotifier) {  
            mQueue.add(i);  
            mNotifier.notifyAll();  
        }  
    }  
}  
public static class Consumer extends Thread {  
    private final ConcurrentLinkedQueue<Integer> mQueue;  
    private final Object mNotifier;  
    public Consumer(ConcurrentLinkedQueue<Integer> queue, Object notifier) {  
        mQueue = queue;  
        mNotifier = notifier;  
    }  
      
    @Override  
    public void run() {  
        while (true) {  
            try {  
                handleOne();  
            } catch (InterruptedException e) {  
                return;  
            }  
        }  
    }  
      
    /**  
     * Somewhere in this function or {@link Producer#sendOne(int)} there is at least one race condition.  
     */    private void handleOne() throws InterruptedException {  
        synchronized (mNotifier) {  
            while (mQueue.isEmpty()) {  
                mNotifier.wait();  
            }  
        }  
        System.out.println("Got data: " + mQueue.poll());  
    }  
}  
  
public static void main(String[] args) {  
    ConcurrentLinkedQueue<Integer> queue = new ConcurrentLinkedQueue<>();  
    Object notifier = new Object();  
    Producer producer = new Producer(queue, notifier);  
    Consumer consumer = new Consumer(queue, notifier);  
      
    producer.start();  
    consumer.start();  
}
```

/*

 * Click `Run` to execute the snippet below!

 */

  

import java.io.*;
import java.util.*;
/*
 * To execute Java, please define "static void main" on a class
 * named Solution.
 *
 * If you need more classes, simply define them inline.
 */


// Given an input of String containing a simple sentense. Can you please write an algorithm to reverse each word of

// the sentence without changing the order of the words in the sentence.

// For e,g. "One simple sentence without complexity" --> "enO elpmis ecnetnes tuohtiw ytixelpmoc"

  

class Solution {
  public static void main(String[] args) {

    String str = "One simple sentence without complexity";
    String res = reverseString(str);
    // System.out.println(res );
    String response = reverseString1(str);

    System.out.println(response );

  }

  

  static String reverseString(String str){
    StringBuilder result = new StringBuilder();
    StringBuilder word = new StringBuilder();
    for(int i=0;i<str.length();i++){
      if(str.charAt(i)!=' '){
        word.append(str.charAt(i));
      }
      else{
        result.append(word.reverse().append(" "));
        word.setLength(0);
      }
    }
    result.append(word.reverse());
    return result.toString();
  }

  static String reverseString1(String str){
    char[] arr = str.toCharArray();
    int start =0;
    for(int i=0;i<=arr.length;i++){
      if(i==arr.length || arr[i]==' '){
        reverse(arr, start, i-1);
        start=i+1;
      }
    }
    return new String(arr);
  }

  static void reverse(char []arr, int start, int end){
    while(start<end){
      char t = arr[start];
      arr[start]=arr[end];
      arr[end]=t;
      start++; end--;
    }
  }
}

  

// Please design a simple database connectivity library where the user can choose depending on requiremnt a choice

// of DBMS to use where we provide say initially 2 implementation and later could add more DB connectivity.

// User must not do any code change just to change DB.

  
  

interface Database{

  void connect();

}

  

class MySqlDatabase implements Database{

  void connect(){}

}

  

class OracleDatabase implements Database{

  void connect(){}

}

  
  
  

class DatabaseFactory{

  public static Database getDatabase(String dty){

    if("MYSQL".equalsIgnoreCase(dty)){

      return new MySqlDatabase();

    }

    else if("ORCALE".equalsIgnoreCase(dty)){

      return new OracleDatabase();

    }

  }

}

  

main(){

Database db = DatabaseFactory.getDatabase("ORACLE");

}

Q. Accenture ask: **Design a simple database connectivity library**

- User can choose DB based on requirement
- Initially support **2 DB implementations**
- Later we should be able to **add more DBs without changing existing code**
- User should not change code just to change DB
## Answer :

👉 **Use Strategy Pattern + Factory Pattern (or DI)**  
This follows **Open–Closed Principle (OCP)** and **Dependency Inversion Principle (DIP)**.

---

## 🔹 Step-by-step Design

### 1️⃣ Create a common interface (Abstraction)

```java
public interface Database {
    void connect();
}
```

---

### 2️⃣ Create concrete implementations

```java
public class MySQLDatabase implements Database {
    @Override
    public void connect() {
        System.out.println("Connected to MySQL");
    }
}
```

```java
public class PostgresDatabase implements Database {
    @Override
    public void connect() {
        System.out.println("Connected to PostgreSQL");
    }
}
```

---

### 3️⃣ Factory to decide which DB to use

```java
public class DatabaseFactory {

    public static Database getDatabase(String dbType) {
        if ("MYSQL".equalsIgnoreCase(dbType)) {
            return new MySQLDatabase();
        } else if ("POSTGRES".equalsIgnoreCase(dbType)) {
            return new PostgresDatabase();
        }
        throw new IllegalArgumentException("Unsupported DB");
    }
}
```

---

### 4️⃣ Client code (User does NOT depend on DB classes)

```java
public class Application {
    public static void main(String[] args) {
        Database db = DatabaseFactory.getDatabase("MYSQL");
        db.connect();
    }
}
```

---

## 🔹 Adding New DB Later (WITHOUT code change in client)

```java
public class OracleDatabase implements Database {
    public void connect() {
        System.out.println("Connected to Oracle");
    }
}
```

➡️ Just update **factory or config**, not business code.

---

## ⭐ Best Enterprise Answer (Extra points)

In **Spring Boot**, this is done via:

- Interface + Implementations
- `@Component`
- `@Qualifier` or config-based selection

Or using **SPI / Dependency Injection**

# Valid parenthesis Problem(Accenture)

#
Q. count total number of prime number in n(wissen)
Q. Nearest largest number(Wissen)


```java
/*  Wissen Tech
Given a m x n grid filled with non-negative numbers,  
find a path from top left to bottom right,  
which minimizes the sum of all numbers along its path.  
Note: You can only move either down or right at any point in time.  
[1,3,1]  
[1,5,10]  
[4,2,1]  
  
output: 9  
*/  
import java.util.*;  
  
public class Main  
{  
    public static void main(String[] args) {        
    int[][]mat = {{1,3,1},                
			    {1,5,10},                
				{4,2,1}};        
				System.out.println(minPathSum(mat, mat.length, mat[0].length));    }  
    public static int minPathSum(int[][]mat, int m, int n) {        
	    int [][]res=new int[m][n];        
	    // if(i==0 && j==0){        
	    //     return mat[0][0];        
	    // }        
	    res[0][0]=mat[0][0];  
        // if(i<0 ||j<0){        
        //     return Integer.MAX_VALUE;        
        // }        
        for(int i=1;i<m;i++){            
	        res[i][0]=res[i-1][0]+mat[i][0];        
	    }        
	    for(int i=1;i<n;i++){            
		    res[0][i]=res[0][i-1]+mat[0][i];        
		}        
		// int top = minPathSum(mat, i-1, j);        
		// int left = minPathSum(mat, i, j-1);  
        // int res = mat[i][j] +Math.min(left, top);        
        for(int i=1;i<m;i++){            
	        for(int j=1;j<n;j++){                
		        res[i][j]=mat[i][j]+Math.min(res[i-1][j], res[i][j-1]);                //System.out.println(res[i][j]);            }  
        }        
        return res[m-1][n-1];    
        }
    }
```

```java
// Wissen tech
public static void main(){  
    List<String> list = Arrays.asList(  
            "abc", "xyx"  
    );  
    list.stream()  
            .collect(Collectors.groupingBy(s->s,  
                     Collectors.counting()))  
            .entrySet()  
            .stream()  
            .filter(c->c.getValue()>1)  
            .collect(Collectors.toMap(  
                    Map.Entry::getKey,  
                    Map.Entry::getValue  
            ));  
  
}
```


## AnthenaHealth
1. Spring boot annotations
2. Threads (focused on asynchronous apis call )
```java

You have a RecentCounter class which counts the number of recent requests within a certain time frame.

Implement the RecentCounter class:

RecentCounter() — Initializes the counter with zero recent requests.  
ping(int t) — Adds a new request at time t, where t represents some time in milliseconds, and returns the number of requests that have happened in the past 3000 milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range [t - 3000, t].  
It is guaranteed that every call to ping uses a strictly larger value of t than the previous call.

Example  
Input:  
["RecentCounter", "ping", "ping", "ping", "ping"]  
[[], [1], [100], [3001], [3002]]

Output:  [null, 1, 2, 3, 3]

package com.assembling;  
  
import java.util.ArrayList;  
import java.util.List;  
  
public class Test6 {  
    static List<Integer> list = new ArrayList<>();  
  
    public static void main(String[]args){  
  
        String [] counterMsg = {"RecentCounter", "ping", "ping","ping", "ping", "ping"};  
        int [] timer = {-1,1,100,3001,3002,9000};  
        for(int i=0;i<counterMsg.length;i++){  
            if(counterMsg[i]=="ping"){  
                System.out.println(ping(timer[i]));  
            }else{  
                list = new ArrayList<>();  
            }  
        }  
  
    }  
  
    public static int ping(int timer){  
        list.add(timer);  
        int left = binarsearch(timer-3000);  
        return list.size()-left;  
    }  
  
    static int binarsearch(int traget){  
        int low =0,  high = list.size()-1;  
        int ans = list.size();  
        while(low<=high){  
            int mid = low+(high-low)/2;  
            if(list.get(mid)>=traget){  
                ans = mid;  
                high = mid-1;  
            }else{  
                low=mid+1;  
            }  
        }  
        return ans;  
    }  
}
```