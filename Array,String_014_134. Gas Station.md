## Array,String_014_134. Gas Station

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return *the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return* `-1`. If there exists a solution, it is **guaranteed** to be **unique**

 

**Example 1:**

```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**Example 2:**

```
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

**Constraints:**

- `n == gas.length == cost.length`
- `1 <= n <= 105`
- `0 <= gas[i], cost[i] <= 104`



## Intuition

**First Understand the problem :**

We have a car with an unlimited gas tank. We are given an array of gas and an array of costs. Each index in the gas array represents the amount of gas available to fill up your car with. Each index in the cost array represents the amount of gas that will be used when travelling from this gas station to next.

Our goal is to determine wether it is possible to start at any of the gas stations and complete one trip around.

If it is, we need to return the index of that starting gas station, if no such gas station exists then we return -1.

Let's understand with an example : We have an **input of gas = [7, 1, 0, 11, 4] & cost = [5, 9, 1, 2, 5]**

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/494f2123-8fcc-455a-8a67-0968223ba270_1642736908.3527038.png)

So, we have **5 gas station** each with the amount of gas value which the amount of gas they can give to your car. Then we have road connecting them which would have a corresponding amount of gas which would be use when your cars travelling.

- So, we start at **1st gas station** with **7 unit of gas**, then make a trip to **2nd gas station** costing **5 unit of gas** which means we have now **2 unit's left** in our tank.
- Upon arriving on next gas station we have given **1 unit of gas** so we have now **3 units**.
- Now we attempt to make a trip to next gas station. However since this trip requires **9 unit of gas**, but we only have **3 in the tank** we run out of gas. So, this means we can start at **gas station at "0"** and make a full trip.

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/a713145b-d63d-4613-b7ba-a589699daadc_1642737420.2182224.png)

- Now lets say we started at **index 3** gas station which gives us **11 units** to start.
- Upon travelling to the next gas station we use **2 units of gas**, but we also get **4 units of gas** upon leaving us with **13 unit of gas** in our tank.
- Now we loop back around and travel back to **0th index** which uses **5 unit of gas,** but we also get refilled of **7 unit of gas** leaving us tank with total of **15**.
- On the next jump, we use **5 unit of gas** and **refuel** for **1** leaving us with **11 unit**.
- On the next jump we use **9 unit of gas**, but we **dont get refuell at all**
- On last trip we use **1 unit of gas**, which leave our tank left with **1 unit of gas**

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/052a8134-f32c-457b-997e-0642798ae5e9_1642743874.4318752.png)
![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/b35476a1-f7bf-4804-adc6-c02290888cac_1642743965.466837.png)
![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/755c9e78-44f3-420c-830a-b2925e76aa66_1642744065.3088975.png)

So, now we know if we start at **index 3** gas station will be able to complete the whole round trip.

## Approach

### 1 Brute Force

#### Intuition

#### Code

```cpp
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        //intuition: brute force, traverse every starting point to check if we can finish our trip
        for (int i = 0; i < gas.length; i++) {
            int totalGas = 0;
            int passedStationCount = 0;
            for (int j = i; passedStationCount < gas.length; passedStationCount++, j++) {
                totalGas += gas[ j % gas.length] - cost[j % gas.length];
                if (totalGas < 0) {
                    break;
                }
            }
            if (passedStationCount == gas.length && totalGas >= 0) {
                return i;
            }
        }
        return -1;
    }
}
```

#### Complexity

- **Time Complexity :-** BigO(N^2)
- **Space Complexity :-** BigO(1)



### 2 Greedy Algorithm

#### Intuition

Find the key points behind the problem description and constraints:

1. If the total amount of gas is less than the total cost required to move between all stations, it's impossible to complete the journey, otherwise, we can definitely find the only feasible starting point, as the description says, If there exists a solution, it is **guaranteed** to be **unique**.
2. Elements of gas and cost array are non-negative, which means if we run out of fuel say at `ith` gas station. All the gas station between `ith and starting point` are unfeasible starting point as well. Because we definitely have positive or zero gas at starting point, which is better then start at the middle point with zero gas.
   So, we can start trying at next gas station on the `i + 1` station, which means we can found the starting point by **O(N) solution**.

#### Code

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalSurplus = 0;
        int surplus = 0;
        int start = 0;
        
        for(int i = 0; i < gas.length; i++){
            totalSurplus += gas[i] - cost[i];
            surplus += gas[i] - cost[i];
          // If we run out of fuel at `ith` gas station
            if(surplus < 0){
              // All the gas station between `ith and starting point` are unfeasible starting point as well. Because we definitely have positive or zero gas at starting point, which is better then start at the middle point with zero gas.
                surplus = 0;
              // We can start trying at next gas station on the `i + 1` station
                start = i + 1;
            }
        }
      // If the total amount of gas is less than the total cost required to move between all stations, it's impossible to complete the journey
      	return (totalSurplus < 0) ? -1 : start;
    }
}
```



#### Detail Expalnation

Let's **Improve this solution** which runs linear in **O(N) time**.

As Inorder to improve the solution we have to look into where it's wasting time

So, our **brute-force** ran a simulation, as soon as a gas station became negative. It's stop and move to the next station as a starting point. But this is inefficient and inorder for us to understand why? we have to look at what makes car stop.
Let's say for this example the **car start at 0** and it's able to make at **3** gas station. And after trying to make it at **4th station** its run out of gas.
![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/bc1d1e8d-66e1-4232-88a6-50ee630652bd_1642739783.4066157.png)

Once the brute force solution realises it can make this trip, it's start over simulating with this gas station as the starting point.
But this next simulation is **useless and wasting time**

**Here's why,**

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/201c0770-bc89-4e1c-8a76-d0af270d86fa_1642740255.742468.png)

Well we already know that these **3 gas stations** and trips **weren't successfull**. Which means comparing our fuel accumulation to our fuel consumption we were at some kind of **surplus** or at the very least we were breaking even with exactly enough fuel to make every trip.

This is because if we were at some kind of deficit then our car would have already run on gas sometime earlier. So, this means on the last trip because we ran out of fuel we were in some kind of deficit.
So, as we can't make the trip **starting at very beginning we can't make over here at index 1 or index 2 or index 3**.

So what does this means in terms of our algorithm, it means that we know if we run out of fuel say at some `ith` gas station. All the gas station between `ith and starting point` are bad starting point as well.
So, this means we can start trying at next gas station on the `i + 1` station. So, hopefully now you understand how this **O(N) solution** will takes place.

#### Complexity

- **Time Complexity :-** BigO(N)
- **Space Complexity :-** BigO(1)