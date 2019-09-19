# #1 Two Sum
```python
class Solution(object):
    def twoSum(self, nums, target):
        if len(nums)<=1:
           return []
        dic = {}
        for i in range(len(nums)):
            if (nums[i] in dic):
                return [dic[nums[i]],i+1]
            else:
                dic[target-nums[i]] = i+1
```

# #2 Add Two Number

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
     def addTwoNumbers(self, l1, l2):
          """
          :type l1: ListNode
          :type l2: ListNode
          :rtype: ListNode
          """
          add = 0
          nxt = 0
          th = None
          if (l1 == None and l2 == None):
               return None
          elif (l1 == None):
               return l2
          elif (l2 == None):
               return l1
          else:
               nxt = l1.val + l2.val
               if (nxt >= 10):
                    add = 1
                    nxt -= 10
               th = ListNode(nxt)
               ret = th
               l1 = l1.next
               l2 = l2.next

          while (l1 != None or l2 != None):
               if (l1 == None):
                    nxt = add + l2.val
                    l2 = l2.next
               elif (l2 == None):
                    nxt = add + l1.val
                    l1 = l1.next
               else:
                    nxt = add + l1.val + l2.val
                    l1 = l1.next
                    l2 = l2.next
               if (nxt >= 10):
                    add = 1
                    nxt -= 10
               else:
                    add = 0
               th.next = ListNode(nxt)
               th = th.next

          if (add == 1):
               th.next = ListNode(add)
          return ret
```

# #4 Median of Two Sorted Arrays

```c
int max(int a,int b){return (a>b)?a:b;}
int min(int a,int b){return (a<b)?a:b;}

double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    int *tmp_ptr;
    int tmp,half_len = (nums1Size+nums2Size+1)>>1,il = 0,ir,ix,jx,retnum1,retnum2;
    
    if (nums1Size>nums2Size){
        tmp_ptr = nums1;
        nums1 = nums2;
        nums2 = tmp_ptr;
        tmp = nums1Size;
        nums1Size = nums2Size;
        nums2Size = tmp;
        tmp_ptr = 0;
    }

    ir = nums1Size;
    while (il<=ir){
        ix = (il+ir)/2;
        jx = half_len - ix;
        if (jx>0 && ix<nums1Size && nums2[jx-1]>nums1[ix]){
            il = ix+1;
        }
        else if (ix>0 && jx<nums2Size && nums1[ix-1]>nums2[jx]){
            ir = ix-1;
        }
        else{
            if (ix == 0){
                retnum1 = nums2[jx-1];
            }
            else if (jx == 0){
                retnum1 = nums1[ix-1];
            }
            else{
                retnum1 = max(nums1[ix-1],nums2[jx-1]);
            }

            if ((nums1Size+nums2Size)&1){
                return (double)retnum1;
            }

            if (ix == nums1Size){
                retnum2 = nums2[jx];
            }
            else if (jx == nums2Size){
                retnum2 = nums1[ix];
            }
            else{
                retnum2 = min(nums1[ix],nums2[jx]);
            }

            return (retnum1+retnum2)/2.0;
        }
    }
}
```

# #6 ZigZag Conversion
```java
import java.util.Arrays;

public class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) return s;
        String str_arr[] = new String[numRows];
        int i = 0,j = 0,len = s.length();
        Arrays.fill(str_arr,"");

        
        while (i < len){
            while (i < len && j < numRows - 1){
                str_arr[j]+=s.charAt(i);
                i++;
                j++;
            }
            
            while(i < len && j > 0){
                str_arr[j]+=s.charAt(i);
                i++;
                j--;
            }
        }
        
        String ret = "";
        for (j = 0;j < numRows;j++){ret+=str_arr[j];}
        return ret;
    }
}
```

# #7 Reverse Integer
```cpp
class Solution {
public:
    int reverse(int x) {
        long long ret = 0;
        bool neg = (x < 0);
        x = abs(x);
        while (x > 0){
            ret = ret * 10 + x % 10;
            x/=10;
        }
        if (((long long)((int)ret)) != ret) return 0;    
        if (neg) ret*=(-1);
        return (int)ret;
    }
};
```

# #9 Palindrome Number
```python
class Solution(object):
    def isPalindrome(self,x):
        if (x < 0 or (x > 0 and x%10 == 0)):
            return False
        tmp = 0
        while (x > tmp):
            tmp = tmp * 10 + x % 10
            if (tmp == x):
                return True
            x = x / 10
        return (tmp == x)
```

# #23 Merge k Sorted Lists
```c
struct ListNode* merge2Lists(struct ListNode* a,struct ListNode *b){
    struct ListNode *ret;
    struct ListNode *fir;
    struct ListNode *sec;
    struct ListNode *tmpfir;
    struct ListNode *tmpfirnxt;
    struct ListNode *tmpsec;
    if (b == 0) return a;
    if (a == 0) return b;
    if (a->val < b->val){
        ret = a;
        fir = a;
        sec = b;
    }
    else{
        ret = b;
        fir = b;
        sec = a;
    }
    
    while (fir != 0 && sec != 0){
        while (fir->next != 0 && fir->next->val < sec->val){
            fir = fir->next;
        }

        if (fir->next == 0){
            fir->next = sec;
            break;
        }

        tmpsec = sec;
        while (tmpsec->next != 0 && tmpsec->next->val < fir->next->val){
            tmpsec = tmpsec->next;
        }

        if (tmpsec->next == 0){
            tmpfirnxt = fir->next;
            fir ->next = sec;
            tmpsec->next = tmpfirnxt;
            break;
        }

        tmpfirnxt = fir->next;
        fir->next = sec;
        sec = tmpsec->next;
        tmpsec->next = tmpfirnxt;
        fir = tmpfirnxt;
    }
    
    return ret;
}


struct ListNode* mergeKLists(struct ListNode** lists, int listsSize) {   
    int i,j,mid;
    while (listsSize > 1){
        mid = (listsSize >> 1);
        i = 0;
        j = mid;
        while (i < mid){
            lists[i] = merge2Lists(lists[i],lists[j]);
            ++i;
            ++j;
        }
        if (listsSize%2 == 0){
            listsSize = mid;
        }
        else{
            lists[mid] = lists[listsSize-1];
            listsSize = mid+1;
        }
    }
    return lists[0];
}
```

# #53 Maximum Subarray
```c
int maxSubArray(int* nums, int numsSize) {
    int i = 0,sum = 0,min = 0,ans = -99999999;
    for (i = 0;i < numsSize;i++){
        sum += nums[i];
        ans = ((sum-min)>ans)?(sum-min):ans;
        min = (sum<min)?sum:min;
    }
    return ans;
}
```

# #56 Merge Intervals
```c
int f[100000],b[100000];
struct Interval* merge(struct Interval* intervals, int intervalsSize, int* returnSize) {
    int i,sum = 0,cnt = -1,max_end = -10000000,min_start = 10000000,started = 0;
    for (i = 0;i < intervalsSize;i++){
        if (intervals[i].start < min_start) min_start = intervals[i].start;
        if (intervals[i].end > max_end) max_end = intervals[i].end;
    }
    
    for (i = min_start;i <= max_end;i++){
        f[i] = 0;
        b[i] = 0;
    }
    
    for (i = 0;i < intervalsSize;i++){
        ++f[intervals[i].start];
        --f[intervals[i].end];
        if (intervals[i].start == intervals[i].end) b[intervals[i].start] = 1;
    }

    for (i = min_start;i <= max_end; i++){
        if (f[i] > 0 && !started){
            started = 1;
            cnt += 1;
            intervals[cnt].start = i;
        }

        if (!started && b[i] == 1){
            cnt += 1;
            intervals[cnt].start = i;
            intervals[cnt].end = i;
        }

        if (started){
            sum += f[i];
            if (sum == 0){
                started = 0;
                intervals[cnt].end = i;
            }
        }
    }

    (*returnSize) = cnt + 1;
    return intervals;
}
```

# #72 Edit Distance
```c
int f[1000][1000];
int min(int a,int b){return (a<b?a:b);}

int minDistance(char* word1, char* word2) {
    int i,j,l1,l2;
    l1 = strlen(word1);
    l2 = strlen(word2);
    if (l1 == 0) return l2;
    if (l2 == 0) return l1;
    for (i = 0;i < l1;i++){
        for (j = 0;j < l2;j++){
            if (i == 0 && j == 0){
                f[i][j] = (word1[i]==word2[j]?0:1);
            }
            else if (i == 0){
                f[i][j] = (word1[i] == word2[j]?j:f[i][j-1] + 1);
            }
            else if (j == 0){
                f[i][j] =(word1[i] == word2[j]?i:f[i-1][j] + 1);
            }
            else{
                f[i][j] = min(f[i-1][j-1]+(word1[i] == word2[j]?0:1),min(f[i-1][j]+1,f[i][j-1]+1));    
            }
        }
    }
    return f[l1-1][l2-1];
}
```


# #99 Recover Binary Search Tree
```c
struct TreeNode* ptr[10000];
int cnt;

void traversal(struct TreeNode* root){
    if (root->left != NULL) traversal(root->left);
    ptr[cnt++] = root;
    if (root->right != NULL) traversal(root->right);
}

void recoverTree(struct TreeNode* root) {
    int i,j,tmp;
    cnt = 0;
    traversal(root);
    for (i = 0;i<cnt && (ptr[i]->val)<=(ptr[i+1]->val);i++){}
    for (j=cnt-1;j>0 && (ptr[j]->val)>=(ptr[j-1]->val);j--){}
    tmp = ptr[i]->val;
    ptr[i]->val = ptr[j]->val;
    ptr[j]->val = tmp;
}

# #135 Candy
```python
class Solution(object):
    def candy(self,ratings):
        array_len = len(ratings)
        if (array_len == 1):
            return 1
        can = []
        result = 0
        for i in range(array_len):
            can.append(1)
        for i in range(1,array_len):
            if (ratings[i] > ratings[i-1]):
                can[i] = can[i-1] + 1
        for i in range(1,array_len):
            if (ratings[array_len-i-1]>ratings[array_len-i]):
                can[array_len-i-1] = max(can[array_len-i-1],can[array_len-i]+1)
        for i in range(array_len):
            result += can[i]
        return result
```

# #164 Maximum Gap
```c
int maximumGap(int* nums, int numsSize) {
    int maxnum,minnum,ind,lst,ans,i,*a,*b;
    double diff;

    if (numsSize < 2){
        return 0;
    }

    a = (int*)malloc(2*numsSize*sizeof(int));
    b = a + numsSize;

    minnum = maxnum = nums[0];
    a[0] = b[0] = -1;
    for (i = 1;i < numsSize;i++){
        maxnum = (nums[i]>maxnum?nums[i]:maxnum);
        minnum = (nums[i]<minnum?nums[i]:minnum);
        a[i] = -1;
        b[i] = -1;
    }
    if (minnum == maxnum) return 0;
    diff = 1.0*(maxnum - minnum)/(numsSize-1);

    
    for (i = 0;i < numsSize;i++){
        ind = (nums[i] - minnum)/diff;
        a[ind] = (a[ind]==-1?nums[i]:(nums[i]<a[ind]?nums[i]:a[ind]));
        b[ind] = (b[ind]==-1?nums[i]:(nums[i]>b[ind]?nums[i]:b[ind]));
    }

    ans = -1;
    for (i = 0;i < numsSize;i++){
        if (b[i] != -1){
            lst = b[i];
            break;
        }
    }
    ++i;
    for (;i < numsSize;i++){
        if (a[i] == -1) continue;
        diff = a[i] - lst;
        ans = (diff>ans?diff:ans);
        lst = b[i];
    }

    free(a);
    return ans;
}
```

# #179 Largest Number
```cpp
class Solution {
public:
private:
    static bool cmp(string a , string b ){
        // return true   a before b
        const char* c = a.c_str();
        const char* d = b.c_str();

        while ((*c)!= '\0' && (*d)!='\0' && (*c)==(*d)){
            ++c;
            ++d;
        }

        if ((*c) == '\0' && (*d) == '\0') return 0;
        if ((*c) == '\0' || (*d) == '\0') return (a+b)>(b+a);
        return ((*c)>(*d));
}
public:
    string largestNumber(vector<int>& nums) {
        int numsSize = nums.size();

        if(nums.empty()){
            return string();
        }
        
        vector<string> num_str;
        //char *p = new char[16];
        for(int i = 0;i < numsSize;i++){
            //itoa(nums[i],p,10);
            //num_str.push_back(string(p));
            num_str.push_back(to_string(nums[i]));
        }
        //delete[] p;
        
        sort(num_str.begin(),num_str.end(),&Solution::cmp);
        

        if(num_str[0]=="0"){
            return "0";
        }

        string ret="";
        for(int i = 0;i < numsSize;i++){
            ret += num_str[i];
        }
        return ret;
    }
};
```

# #200 Number of Islands
```cpp
#include <vector>
#include <queue>
using namespace std;
const int dir[4][2] = {{0,1},{0,-1},{1,0},{-1,0}};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;
        length = grid.size();
        if (length == 0){
            return 0;
        }
        width = grid[0].size();
        if (width == 0){
            return 0;
        }
        for (int i = 0;i < length;i++){
            for (int j = 0;j < width;j++){
                if (grid[i][j] == '1'){
                    ++ans;
                    grid[i][j] = '0';
                    dfs(grid,i,j);
                }
            }
        }
        return ans;    
    }

private:
    int length,width;
    void dfs(vector<vector<char>>& grid,int x,int y){
        for (int i = 0;i < 4;i++){
                int dir_x = x + dir[i][0];
                int dir_y = y + dir[i][1];
                if (dir_x < 0 || dir_x >= length || dir_y < 0 || dir_y >=width){
                    continue;
                }
                if (grid[dir_x][dir_y] == '0'){
                    continue;
                }
                grid[dir_x][dir_y] = '0';
                dfs(grid,dir_x,dir_y);
        
        }
    }
};
```

# #210 Course Schedule II
```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<int> result;
        vector<int> in_degree(numCourses,0);
        vector<pair<int,int>>::iterator iter;
        result.clear();

        for (iter = prerequisites.begin();iter != prerequisites.end();iter++){
            ++(in_degree[iter->first]);
        }

        for (int i = 0;i < numCourses;i++){
            if (in_degree[i] == 0){
                result.push_back(i);
                in_degree[i] = -1;
                
                iter = prerequisites.begin();
                while (iter != prerequisites.end()){
                    if (iter->second == i){
                        --(in_degree[iter->first]);
                        iter = prerequisites.erase(iter);
                    }
                    else{
                        ++iter;
                    }
                }
                
                i = -1;
            }    
        }
    
        if (result.size() < numCourses){
            result.clear();
        }
        return result;
    }
};
```

# #263 Ugly Number
```python
class Solution(object):
    def isUgly(self, num):
        if (num <= 0):
            return False
        while (num % 2 == 0):
            num = num / 2
        while (num % 3 == 0):
            num = num / 3
        while (num % 5 == 0):
            num = num / 5
        if (num == 1):
            return True
        else:
            return False
```

# #264 Ugly Number II
```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        if (n == 0) return 0;
        vector<int>res;        
        res.clear();
        res.push_back(1);
        int t2(0),t3(0),t5(0),k(1);
        while (k != n){
            res.push_back(min(res[t2]*2,res[t3]*3,res[t5]*5));
            if (res[k] == res[t2]*2) ++t2;
            if (res[k] == res[t3]*3) ++t3;
            if (res[k] == res[t5]*5) ++t5;
            ++k;
        }
        return res[k-1];
    }
private:
    int min(int a,int b,int c){
        int ret = (a<b)?a:b;
        return ((c<ret)?c:ret);    
    }
};
```
