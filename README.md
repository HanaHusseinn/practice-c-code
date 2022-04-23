# practice-c-code

##  1️⃣ Find a pair with the given sum in an array

### SOLUTION 1: USING SORTING

🟧 **Libraries**
```
#include <iostream>
#include <algorithm> //for sort

using namespace std;
```


🟧 **Function: findPairs (The function that will be called in main)**


```
//assuming positive numbers 
void findPairs(int arr [], int arrLength, int targ)
{
    sort(arr,arr+arrLength);
    int low=0, high = arrLength-1;
    bool pairFound=false;

    //While cases:
    while ( high>low ) //this means if they are on the same number even if 5 which its sum is 10,
                       //still wont be written which is right bec its not a pair
    {
        
        //Case1: find pairs of target
        if (arr[low]+arr[high]==10)
        {
            cout<<"pair is: ("<<arr[low]<<","<<arr[high]<<")"<<endl;
            low++;high--;
            //return;
            pairFound=true;
        }
    
        //Case 2 & 3: if sum larger than targ, reduce high else inc low to enlarge the number
        else
            arr[low]+arr[high]>10 ? high--: low++;
            
        //Case 4: no pairs found

    }
    if (pairFound==false)
        cout<<"No pairs found";
    
}
```

🟧 **Function: main**
```
int main()
{
    int nums [] = {1,5,7};//{9,2,4,6,1,8,0,20,10,5,7};
    int target = 10;
    //Note: n should be calculated here and passed as a parameter to the function bec the function takes arr 
    //      as a parameter and won't be able t calc its size in the func bec size of ptr to array is address size 
    //      which is 8 bits or 2 bytes    
    //Summary: I can't find the size of an array using a pointer which only caries address to its 1st element
    int n= sizeof(nums)/sizeof(nums[0]) ; //or *(&arr+1)-arr
    findPairs(nums,n,target);
    return 0;
}
```

🟥**Output:** 

🟩 Case 1 : nums= {9,2,4,6,1,8,0,20,10,5,7}

```
pair is: (0,10)
pair is: (1,9)
pair is: (2,8)
pair is: (4,6)
```
🟩 Case 2 : nums={1,5,7}

```
No pairs found
```
|Advantages (Space)| Disadvantages (Complexity)|
|------|-------|
|No extra space needed to the size of input array| O(n.log(n))|

-----------------------------------------------------------------------------------------------------------------------------------------------------------

### SOLUTION 2: USING MAP HASHING

🔴🔴🔴🔴🔴🔴🔴🔴🔴 **Useful tutorial on Unordered Maps** 🔴🔴🔴🔴🔴🔴🔴🔴🔴
```
#include <iostream> //for cin,cout,endl
#include <unordered_map> //for unordered_map,pair
#include <algorithm> //for for_each
#include <string> //for string

int main()
{
    std::unordered_map<std::string, int> WordMap ({{"First",1},{"Second",2},{"Third",3}});
    
    for (std::pair<std::string,int> element: WordMap)
    {
        std::cout<<element.first<< " :: " <<element.second << std::endl;
    }
    
    std::cout<<"************"<<std::endl;
    
    std::unordered_map<std::string,int>:: iterator it = WordMap.begin();
    while (it!=WordMap.end())
    {
        std::cout<<it->first<<" :: "<<it->second<<std::endl;
        it++;
    }
    
    std::for_each(WordMap.begin(),WordMap.end(),[](std::pair<std::string,int>element)
    {
        std::cout<<element.first<<" :: "<<element.second<<std::endl;
    });
    
    
    return 0;
}
```
🔴🔴🔴🔴🔴🔴🔴🔴🔴

💎**Idea** 

to check previous elements in array from a given location, use hash which is much faster than iterating

unordered map uses find function which is faster than iterating all previous

so for each element, if i dont find its pair in previous elements, add it to the map

hoping that i find its pair in the future in the rest of the upcoming elements,

so when i check back the previous elements, I find the pair that I preiously stored(when I didn't find its pair)

then remove the pair from the map to reduce the next finding size in the map for the found pair

as it won't repeat the pairs(each number is written once in the array)


🟧 **Libraries**

```
#include <iostream>
#include <unordered_map>
```
🟧 **Function: findPairs (The function that will be called in main)**
```
void findPairs(int arr [],int size, int target)
{
    std::unordered_map<int,int> WordMap;
    bool pairFound = false;
    //in the map, i will put the no. in nums arr ad its sum complement
    for (int i =0 ; i < size; i++)
    {
        if (WordMap.find(target-arr[i])!=WordMap.end()) //if its sum complement to target is seen before
        //then print it and its pair(sum complement to target) that was previously stored
        {
            std::cout<<"pair is: ("<<target-arr[i]<<","<<arr[i]<<")"<<std::endl;
            //fo rtarget=10, even if 5 is the number iteration, still will find it once and then no other 5 is found in the rest of the numbers, so wont get a pair to it
            WordMap.erase(target-arr[i]); //useful for when th map is large and large numbers are stored for sum of 10000 for ex.
            //so delete one by one at its time to reduce the steps and itertions in find and remove the already found vals
            pairFound= true; //if i will erase, 
            //how to do it once?
        }
            
        else
        //add the current element(which will act as sum complement to target to one of the next elements, if found)
            WordMap[arr[i]]=i; //this i is any value, just to use the map for faster search among vals
    }
    
    //if (WordMap.begin()==WordMap.end()) //will work if i dont do the erase line
    if (pairFound==false)
        std::cout<<"no pairs found";
    
}
```
🟧 **Function: main**
```
int main()
{
    int nums []= {9,2,5,0,33,1,6,7,3,4,8,20,10}; //{5,1,7}
    int n = sizeof(nums)/sizeof(nums[0]);
    int target = 10;
    findPairs(nums,n,target);
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= {9,2,5,0,33,1,6,7,3,4,8,20,10}

```
pair is: (9,1)
pair is: (7,3)
pair is: (6,4)
pair is: (2,8)
pair is: (0,10)
```
🟩 Case 2 : nums={5,1,7}

```
no pairs found
```
|Advantages (Complexity)| Disadvantages (Complexity)|
|------|-------|
|O(n) | requires O(n) extra space, where n is the size of the input|

-----------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------

##  2️⃣ Print all subarrays with 0 sum

💎**Idea**

first i think about using maps for faster find

i will write the sum of values starting from 'each index' till the last element in array

if the sum is 0, at any point, this means that from THAT index, till this point: subarray sum=0

so ex:

{4,2,-3,-1,0,4}  -> 5 indices
|index|value|
|------|----|
|0|4|
|1|2|
|2|-3|
|3|-1|
|4|0|
|5|4|

|start index/all the indices|0|1|2|3|4|5|
|------|-------|--|--|--|--|--|
|0|(0,4)||||||
|1|(1,6)|(1,2)||||
|2|(2,3)|(2,-1)|(2,-3)||||
|3|(3,2)|(3,-2)|(3,-4)|(3,-1)|||
|4|(4,2)|(4,-2)|(4,-4)|(4,-1)|[![](https://img.shields.io/badge/(4,0)-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)||
|5|(4,6)|(5,2)|[![](https://img.shields.io/badge/(5,0)-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(5,-3)|(5,4)|(5,4)|

so according to the idea: 

starting from index 2 till index 5 -> sum = 0 , which means the elements is no. at index [2,5] where their sum = 0 ---> subarr={-3,-1,0,4}

starting from index 4 till index 4 -> sum = 0 , which means the only element is no. at index no.4 whose val = 0 ---> subarr={0}

But take care that means for each idex i will have to calc sum starting from it till all the next elemetns till the end

this is a redundant problem bec i already cal sum of all eelemts form the 1st index, 

so if i want starting from the second element index, i will only subtract the first element value from all the next values

which will mean im starting from the second value, and thenn again find the index at which sum = 0, 

it will mean sb array startig from 2nd element till this index, sum=0

BUT again, this is not good to keep looping for each element and subtracting the previous values from them

as if we look close to the values in the table we will find that the subarrays that have sum = 0,

they are eq to having the sum at the index is repeated at a later index which means all the elemnts in betwee, their sum =0

it is equivalent to if we start at an index till we get zero


|start index/all the indices|0|1|[![](https://img.shields.io/badge/2-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|3|4|5|
|------|-------|--|--|--|--|--|
|0|(0,4)||||||
|1|(1,[![](https://img.shields.io/badge/6-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant))|(1,2)||||
|[![](https://img.shields.io/badge/2-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(2,3)|(2,-1)|(2,-3)||||
|[![](https://img.shields.io/badge/3-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(3,2)|(3,-2)|(3,-4)|(3,-1)|||
|[![](https://img.shields.io/badge/4-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(4,2)|(4,-2)|(4,-4)|(4,-1)|(4,0)||
|[![](https://img.shields.io/badge/5-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(4,[![](https://img.shields.io/badge/6-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant))|(5,2)|[![](https://img.shields.io/badge/(5,0)-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(5,-3)|(5,4)|(5,4)|

6,6 is repeated back so if we start at index after it which is 2 till 5, sum of all these elements =0

|start index/all the indices|0|1|2|3|[![](https://img.shields.io/badge/4-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|5|
|------|-------|--|--|--|--|--|
|0|(0,4)||||||
|1|(1,6)|(1,2)||||
|2|(2,3)|(2,-1)|(2,-3)||||
|3|(3,[![](https://img.shields.io/badge/2-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant))|(3,-2)|(3,-4)|(3,-1)|||
|[![](https://img.shields.io/badge/4-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)|(4,[![](https://img.shields.io/badge/2-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant))|(4,-2)|(4,-4)|(4,-1)|[![](https://img.shields.io/badge/(4,0)-blue?style=for-the-badge)](https://github.com/hamzamohdzubair/redant)||
|5|(4,6)|(5,2)|(5,0)|(5,-3)|(5,4)|(5,4)|

2,2 is repeated back so if we start at index after it which is 4 till 4, sum of all these elements =0

We can notice from the prvious tables that

usig the turnaround of that if we find a sum is repeated, then starting from that index till the current one, all the leemnt in betwee, sum=0

(which we used insted of subr=tracting vlaues everytime to ocheck each index)

so **find the sum repeated back-> subarray = the region in between (start,end]**, we **take the start as the next index(add +1)** to the 'found' index which has the same sum

as we see, we check the element previous the the atual start index to check the sum value

so we need an index before the actual start index of the subarray range

NOTE: if the sum starts at 1st element,  we need to check sum at its previous position (-1) to know if really sum previous to it is 0

(0 bec starting at 1st pos =3 for ex -> sum=3 till that index , 2nd pos = -2 -> sum = 0 (till taht pos) bec we start at the index so we get th 0, which we wanted to get startnig fromt he other indices but we did it withha turnaround)

so we have to **insert sum = 0 at pos -1** to be checked

### SOLUTION : USING MULTIMAP HASHING
🟧 **Libraries**
```
#include <iostream>
#include <unordered_map>
```
🟧 **Function: findSubarrayZeroSum (The function that will be called in main)**
```
void findSubarrayZeroSum(int arr [], int size)
{
    std::unordered_multimap<int, int> subarrayMap;
    //since sum can be found at many locations, and still it is the key, so we use multimap that allows same key(sum) to be repeated at many locations
    //unlike unordered_map where the key can't be repeated
    
    subarrayMap.insert(std::pair<int,int> (0,-1));
    
    int sum =0;
    bool subarrayFound=false;
    
    for (int i =0 ; i<size ; i++)
    {//key is sum, its at first pos
        sum += arr[i];
    
        if (subarrayMap.find(sum)!=subarrayMap.end()) //found first previous loc with same sum, so all in bet sum=0
        //since we want to find the locations with certain sum, then sum is the key,then it is the first part of the pair
        {
            subarrayFound=true;
            std::unordered_multimap<int,int>::iterator it = subarrayMap.find(sum);
            
            //find all next locations that have the sum
            while (it!=subarrayMap.end() && it->first==sum)
            {
                int fist_arr_index=it->second +1;
                
                std::cout<<"Subarray = { ";
                for (int element = fist_arr_index;element<i;element++)
                {
                    std::cout<<arr[element]<< " , ";
                }
                std::cout<<arr[i]<<" }"<<std::endl;;

                it++;
            }
            //for (int element: arr)    
        }
        subarrayMap.insert(std::pair<int,int> (sum,i));
    }
    if (subarrayFound==false)
        std::cout<<"No subarrays of 0 sum are found";    
}
```
🟧 **Function: main**
```
int main()
{
    int nums [] = {4,2,-3,-1,0,4};
    int n= sizeof(nums)/sizeof(nums[0]);
    findSubarrayZeroSum(nums,n);
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= { 4, 2, -3, -1, 0, 4 }

```
Subarray = { 0 }
Subarray = { -3 , -1 , 0 , 4 }
```
🟩 Case 2 : nums= { 3, 4, -7, 3, 1, 3, 1, -4, -2, -2 }

```
Subarray = { 3 , 4 , -7 }
Subarray = { 4 , -7 , 3 }
Subarray = { -7 , 3 , 1 , 3 }
Subarray = { 3 , 1 , -4 }
Subarray = { 3 , 1 , 3 , 1 , -4 , -2 , -2 }
Subarray = { 3 , 4 , -7 , 3 , 1 , 3 , 1 , -4 , -2 , -2 }
```
🟩 Case 3 : nums= { 3, 4, 7,-1,6 }

```
No subarrays of 0 sum are found
```
Advantage: not looping from each index till end of array, redundant info stored in the summation of the only first index till the last element
and hust checking the repeated sum to indicate range in between is zero, and start index is the stored index so loop on all of them after 'find' the first one, and the last index is the current index location in the loop

-----------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------

## 3️⃣ Sort binary array in linear time

### SOLUTION 1: USING COUNT (not O(n)) - My solution

🟧 **Libraries**
```
#include <iostream>
```
🟧 **Function: sortBinary (The function that will be called in main)**
```
int* sortBinary(int arr [],int size)
{
    int countZero =0, countOne=0;
    
    for (int i = 0 ; i<size; i++)
    {
        if (arr[i]==0)
            countZero++;
        else
            countOne++;
    }
    
    for (int i =0 ; i<countZero; i++)
        arr[i]=0;
    
    for (int i =countZero ; i<(size) ; i++) //size is same as countZero+countOne
    //so actually i dont need to count ones, just zeros and fill the rest from after 0 till the size with 1
        arr[i]=1;
    
    return arr;
}
```
🟧 **Function: print (The function that will be called in main)**
```
void print(int arr [], int size)
{
    std::cout<<"Sorted Array = { ";
    
    for (int i =0;i<size-1;i++)
        std::cout<<arr[i]<<" , ";
        
    if (size!=0)
        std::cout<<arr[size-1];
    
    std::cout<<" }"<<std::endl;
}

```
🟧 **Function: main**
```
int main()
{
    int nums [] = {1,1,1,1,1,0};
    int n=sizeof(nums)/sizeof(nums[0]);
    int * arr = sortBinary(nums,n);
    
    print(arr, n);
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= { 1, 0, 1, 0, 1, 0, 0, 1 }

```
Sorted Array = { 0 , 0 , 0 , 0 , 1 , 1 , 1 }
```
🟩 Case 2 : nums= {}

```
Sorted Binary = {  }
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------
### SOLUTION 2: INSTANTLY FILL THE 0 IN ITS POSITION TO OVERWRITE 1 NOT COUNT FIRST THEN FILL
🟧 **Libraries**
```
#include <iostream>
```
🟧 **Function: sortBinary (The function that will be called in main)**
```
int* sortBinary(int arr [], int size)
{
    int nextZeroPos=0;
    for(int i =0;i<size;i++)
    {
        if(arr[i]==0) //instatnly fill the zero in its position to overwrite 1, not swap, so we still do another for loop for the ones
        //also this checks all the n elements, then do another for loop for the ones
            arr[nextZeroPos++]=0; // arr[nextZeroPos]=0; nextZeroPos++
    }
    
    for (int i= nextZeroPos; i<size; i++)
        arr[i]=1;
    
    return arr;
}
```
🟧 **Function: print (The function that will be called in main)**
```
void print(int arr [], int size)
{
    std::cout<<"Sorted Binary = { ";
    for (int i=0; i<size-1; i++)
        std::cout<<arr[i]<<", ";
    std::cout<<arr[size-1]<<" }"<<std::endl;
}
```
🟧 **Function: main**
```
int main ()
{
    int nums[] = {1,0,1,0,1,0,1,1,0,1,0,0};
    int n= sizeof(nums)/sizeof(nums[0]);
    
    int* sortedArr= sortBinary(nums,n);
    print(sortedArr,n);
    
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= { 1, 0, 1, 0, 1, 0, 0, 1 }

```
Sorted Array = { 0 , 0 , 0 , 0 , 1 , 1 , 1 }
```
🟩 Case 2 : nums= {}

```
Sorted Binary = {  }
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------
### SOLUTION 3: QUICKSORT PARITION: USING 'PIVOT' AND INSTANTLY 'SWAP' TO 'NEXT ZERO POSITION' TO FILL BOTH 0,1 AT THE SAME TIME
🟧 **Libraries**
```
#include <iostream>
```
🟧 **Function: swap (The function that will be called in partition)**
```
void swap(int * arr, int i, int j) //ptr to first element which is easily accessed by [],
//but if it was array of adresses (&arr) , then should've done *(&arr) to get address of first element
//and then [] to access the element
{
    int temp=arr[i];
    arr[i]=arr[j];
    arr[j]=temp;
}
```
🟧 **Function: partition (The function that will be called in main)**
```
int* partition (int arr [], int size)
{
    int pivot = 1;
    int nextZeroPos=0;
    
    for (int i =0 ; i< size; i++)
    {
        if (arr[i]< pivot) //=0
        {
            swap(arr,i,nextZeroPos);
            nextZeroPos++;
            
        }
    }

    return arr;//arr asln is a paramater as a pointer to only the 1st element in arr, it's of size 8 bytes, 
    //so it returns as int*
    //arr[0]= its value 
}
```
🟧 **Function: print (The function that will be called in main)**
```
void print(int arr [], int size)
{
    std::cout<<"Sorted Binary = { ";
    for (int i=0; i<size-1; i++)
        std::cout<<arr[i]<<", ";
    std::cout<<arr[size-1]<<" }"<<std::endl;
}
```
🟧 **Function: main**
```
int main ()
{
    int nums[] = {1,0,1,0,1,0,1,1,0,1,0,0};
    int n= sizeof(nums)/sizeof(nums[0]);
    int * sortedArr=partition(nums,n);
    print(sortedArr,n); 
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= { 1, 0, 1, 0, 1, 0, 0, 1 }

```
Sorted Array = { 0 , 0 , 0 , 0 , 1 , 1 , 1 }
```
🟩 Case 2 : nums= {}

```
Sorted Binary = {  }
```

**Note**: std::sort function in 'algorithm' library is of n log (n) time complexity so it doesn't work with this example

**Advantage**: Better Method because it swaps the 0,1 at once

-----------------------------------------------------------------------------------------------------------------------------------------------------------
### EXTRA (RELATED QUESTION): Rearrange even and odd numbers in an array in linear time such that all even numbers come before all odd numbers.

💎**Idea:**

We can't use the first two methods:

1- We can't use the 1st method: bec the numbers are not same so we cnt depend on knowing the count of a number and filling the other(ex: 2,4,3,6,5) so we cant just count how many even no. then fill, and also we dont know the rest of odd numbers to fill after

2- We can't use the 2nd method: bec (even if for every even number we meet on iterating, we place the even number in its next position at the beginning), this means we are overwriting the oddnnumbers, and also we will lose them and can't just fill them after the even numbers with a certain single number

3- We can only use the third method: to swap at once any even number to be at the beginning and replace its position with the odd number

**SOLUTION** : QUICKSORT PARITION: USING 'PIVOT' AND INSTANTLY 'SWAP' TO 'NEXT ZERO POSITION' TO FILL BOTH 0,1 AT THE SAME TIME

🟧 **Libraries**
```
#include <iostream>
```
🟧 **Function: swap (The function that will be called in partition)**
```
int* swap (int arr [], int i, int j)
{
    int temp=arr[i];
    arr[i]=arr[j];
    arr[j]= temp;
    
    return arr;
}
```
🟧 **Function: partition (The function that will be called in main)**
```
int* partition(int arr [], int size)
{
    int nextEvenPos = 0;
    for (int i =0; i<size; i++)
    {
        if (arr[i]%2==0)//even number
        {
            swap(arr,i,nextEvenPos);
            nextEvenPos++;
        }
    }
    
    return arr;
}
```
🟧 **Function: partition (The function that will be called in main)**
```
void print(int arr [], int size)
{
    std::cout<<"Sorted Even Before Odd = { ";
    for (int i=0; i<size-1; i++)
        std::cout<<arr[i]<<", ";
    std::cout<<arr[size-1]<<" }"<<std::endl;
}
```
🟧 **Function: main**
```
int main ()
{
    int nums[] = {5,6,1,3,8,2,10,20,30,33,40,54,55,71,89,80};
    int n= sizeof(nums)/sizeof(nums[0]);
    
    int* sortedArr= partition(nums,n);
    print(sortedArr,n);
    
    return 0;
}
```
🟥**Output:** 

🟩 Case 1 : nums= {5,6,1,3,8,2,10,20,30,33,40,54,55,71,89,80}

```
Sorted Even Before Odd = { 6, 8, 2, 10, 20, 30, 40, 54, 80, 33, 3, 5, 55, 71, 89, 1 }
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------

### Comparison on the 3 solution

We have two categories (either each category is of the same number or a group of common things)

ex:

| | example|1st category| 2nd category|
|--|--------|-----------|-------------|
|same number/cateogry|{zeros, ones}| {0,0,0,0,0,0,0}|{1,1,1,1,1,1,1}|
|group of diffenet values of common feature/category|{even,odd}|{2,6,40,102,44,12,8}|{3,51,67,891,33,1,27}|

Solutions for the two cases:

|same number/category(gp)|diff values (of same featutre) in the group|
|------|----|
|1- choose a no. as pivot, for loop to count the pivot occurences , for loop to fill it with the count, another for loop till the end of the array to fill it with the other number|    X |
|2- for loop to instantly fill the pivot number (whenever i find it through the loop) in its next pos from beginning to overwrite the other number, for loop to fill the aarray till the end with the other number   | X |
|3- QuickSort technique that uses partiton to two groups technique, choose pivot(numbers to be in the 1st group) and for loop to whenevr i find a pivot, swap it with its next pos from the begiining with the non pivot number (but not overwrite it) a |   SAME  |

-----------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------

