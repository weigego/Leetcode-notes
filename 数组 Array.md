# 数组

#### [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)  [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

暴力解法：三层for，复杂度 $O(N^3)$，运行超过时间限制。

优化解法：

1. 按升序排序数组 时间复杂度$O(NlogN)$，空间复杂度$O(logN)$ (quick sort)
2. 假设答案为 (a,b,c)，先固定第一位 a （一层for循环）
3. 再找利用双指针（一首(指针b)一尾(指针c)）找满足条件的b, c
   1. 如 `a+b+c < target`， b++
   2. 如 `a+b+c > target`， c--

15题解法：

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans={};
        sort(nums.begin(),nums.end());
        int size = nums.size();
        for (int a = 0; a < size-2; a++){
            if (nums[0]>0) break; //升序排列数组nums，如果第一位已经是正数，则不可能有和为0的三个数
            if (a>0 && nums[a]==nums[a-1]) { //如果nums[a]和num[a-1]的一样，则a++，避免出现重复的三位数
                continue;
            }
            int b = a+1, c = size-1;
            while (b<c){
                int sum = nums[a]+nums[b]+nums[c];
                if (sum == 0) {
                    ans.push_back({nums[a],nums[b],nums[c]});
                    b++; //继续检查有无符合的答案 （这一行也可替换为c--)
                }else if (sum <0){
                    b++;
                }else{ //sum>0 
                    c--;
                }
                while (c+1<size && a<c && nums[c]==nums[c+1]) c--; //找出和之前不重复的nums[c]
                while (b-1>a && b<size && nums[b]==nums[b-1]) b++; 
            }
        }
        return ans;

    }
};
```



16 题解法:

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int size = nums.size();
        int ans = nums[0]+nums[1]+nums[2]; 
        for (int i = 0; i < size-2; i++ ){
            int low = i+1, high= size-1;
            while (low<high){ 
                int sum = nums[i] + nums[low]+nums[high];
                if (sum == target){
                    return target;
                }
                if (abs(sum - target) <= abs(ans - target)){
                    ans = sum;                                        
                }    
                if (sum < target){ 
                    low++;     
                }else{ //(sum > target)
                    high--;
                }
            }
        }
        return ans;

    }
};
```
