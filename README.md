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

### SOLUTION 2: USING HASHING

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
        if (WordMap.find(target-arr[i])!=WordMap.end()) //not seen before
        {
            std::cout<<"pair is: ("<<target-arr[i]<<","<<arr[i]<<")"<<std::endl;
            //fo rtarget=10, even if 5 is the number iteration, still will find it once and then no other 5 is found in the rest of the numbers, so wont get a pair to it
            WordMap.erase(target-arr[i]); //useful for when th map is large and large numbers are stored for sum of 10000 for ex.
            //so delete one by one at its time to reduce the steps and itertions in find and remove the already found vals
            pairFound= true; //if i will erase, 
            //how to do it once?
        }
            
        else
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

##  2️⃣ Find a pair with the given sum in an array

