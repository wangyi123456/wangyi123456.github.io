# Java-Interview------Top coding
[TOC]

## 排序与查找算法：

#### 排序算法

关于排序和查找算法的考察，在校招面试和社招面试中都是极其热门的，不外乎别的，仅仅是因为这就是最基础并且常用的算法。

**放水排序：选择排序和冒泡排序**

都是O(n^2)的稳定排序，选择排序是每次遍历找出最小的数放在最左边，冒泡排序是找到最大的数放到最右边

```java
public static int[] selectSort(int[] arr) {
    for (int i =0; i<arr.length-1; i++){
        int mini = i;
        for (int j=i+1;j<arr.length;j++) {
            if (arr[j]<arr[mini]) mini = j;
        }
        swap(arr,mini,i);
    }
    return arr;
}
```

```java
public static int[] bubbleSort(int[] arr) {
        if (arr ==null || arr.length<2) return arr;
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr.length - i - 1; j++) {
                if (arr[j+1]<arr[j]) swap(arr,j,j+1);
            }
        }
        return arr;
    }
```

**插入排序**

```java
    public static int[] insertSort(int[] arr) {
        if (arr ==null || arr.length<2) return arr;
        for (int i = 1; i < arr.length; i++) {
            int tem = arr[i];
            int k = i - 1;
            while (k >= 0 && arr[k] > tem) {
                k--;
            }
//           腾出位置插进去,要插的位置是 k + 1; 
            for (int j = i; j > k + 1; j--) {
                arr[j] = arr[j - 1];
            }
//            插入
            arr[k+1] = tem;
        }
        return arr;
    }
```

**快速排序:**

- 必须先从右边走起，
- 除了大小比较其余全没有等于号，
- partition开始要记录下左边界，交换完后放到中间

```java
public static void quickSort(int[] arr, int l, int r) {
    if (l < r) {
        int par = partition(arr, l, r);
        quickSort(arr, l, par - 1);
        quickSort(arr, par + 1, r);
    }
}
public static int partition(int[] arr, int l, int r) {
    int tem = l;
    while (l < r) {
        while (l < r && arr[r] >= arr[tem]) r--;
        while (l < r && arr[l] <= arr[tem]) l++;
        if(l<r) swap(arr,l,r);
    }
    swap(arr,tem,l);
    return l;
}
public static void swap(int[] arr, int a, int b) {
    int t = arr[a];
    arr[a] = arr[b];
    arr[b] = t;
}
```

**归并排序**

```java
public static int[] mergeSort(int[] arr, int l, int r) {
    if (l < r) {
        int mid = (l+r)/2;
        arr = mergeSort(arr, l, mid);
        arr = mergeSort(arr, mid + 1, r);
        merge(arr,l,mid,r);
    }
    return arr;
}
private static void merge(int[] arr, int left, int mid, int right) {
    int[] a = new int[right + 1 - left];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (arr[i] < arr[j]) a[k++] = arr[i++];
        else a[k++] = arr[j++];
    }
    while (i <= mid) a[k++] = arr[i++];
    while (j <= right) a[k++] = arr[j++];
    for (int m = 0; m < k; m++) {
        arr[left++] = a[m];
    }
}
```

#### 快排兄弟——[最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

```python
class Solution:
    def getLeastNumbers(self, arr: List[int], k: int) -> List[int]:
        if k >= len(arr): return arr
        def quick_sort(l, r):
            i, j = l, r
            while i < j:
                while i < j and arr[j] >= arr[l]: j -= 1
                while i < j and arr[i] <= arr[l]: i += 1
                arr[i], arr[j] = arr[j], arr[i]
            arr[l], arr[i] = arr[i], arr[l]
            if k < i: return quick_sort(l, i - 1) 
            if k > i: return quick_sort(i + 1, r)
            return arr[:k]
            
        return quick_sort(0, len(arr) - 1)
```

为对比学习列出[寻找第K大](https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=117&&tqId=37791&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)的代码：

```python
class Solution:
    def findKth(self, arr, n, k):
#         if k >= len(arr): return arr
        def quick_sort(l, r):
            i, j = l, r
            while i < j:
                while i < j and arr[j] <= arr[l]: j -= 1
                while i < j and arr[i] >= arr[l]: i += 1
                arr[i], arr[j] = arr[j], arr[i]
            arr[l], arr[i] = arr[i], arr[l]
            if k-1 < i: return quick_sort(l, i - 1) 
            if k-1 > i: return quick_sort(i + 1, r)
            return arr[k-1]
        return quick_sort(0, len(arr) - 1)
```

#### 二分查找算法：

本文对应的力扣题目：

[704.二分查找](https://leetcode-cn.com/problems/binary-search)

[34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

1.6.1 二分查找框架

```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;
    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```

1.6.2 寻找一个数（基本的二分搜索）

这个场景是最简单的，可能也是大家最熟悉的，即搜索一个数，如果存在，则返回其索引，否则返回 -1。

```java
int binarySearch(int[] nums, int target) {
    int left = 0; 
    int right = nums.length - 1; // 注意

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target)
            return mid; 
        else if (nums[mid] < target)
            left = mid + 1; // 注意
        else if (nums[mid] > target)
            right = mid - 1; // 注意
    }
    return -1;
}
```

1.6.3 寻找左侧边界的二分搜索

以下是最常见的代码形式，其中的标记是需要注意的细节：

```java
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0;
    int right = nums.length; // 注意
    
    while (left < right) { // 注意
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid; // 注意
        }
    }
    return left;
}
```

1.6.4 寻找右侧边界的二分查找

比较常见的左闭右开的写法，只有两处和搜索左侧边界不同，已标注：

```java
int right_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1; // 注意
}
```

**我们还根据逻辑将「搜索区间」全都统一成了两端都闭，便于记忆，只要修改两处即可变化出三种写法**：

```java
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，收紧右边界，锁定左侧边界
            right = mid - 1;
        }
    }
    // 最后要检查 left 越界的情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}


int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，收紧左边界，锁定右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
```

## 数组相关的算法：

数组是一种极其常见的数据结构，在面试中和数组相关的算法题出现频率相当高。对数组相关算法题目的考察，一般对时间复杂度和空间复杂度有所要求。所以，最常用的方法就是设置两个指针，分别指向不同的位置，不断调整指针指向来实现O（N）时间复杂度内实现算法。常见的面试题目：

#### [两数之和](https://www.nowcoder.com/practice/20ef0972485e41019e39543e8e895b7f?tpId=117&&tqId=37756&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

```python
class Solution:
    #     解法1 暴力解法
    def twoSum(self , numbers , target ):        
        for i in range(len(numbers)):
            for j in range(i+1,len(numbers)):
                if(numbers[i]+numbers[j] == target):return[i+1,j+1]
                
#     解法2 ：空间换时间
    def twoSum(self , numbers , target ):
        Hmap = dict()
        for i,num in enumerate(numbers):
            Hmap[num] = i
        for i in range(len(numbers)):
            index = target - numbers[i]
            if index in Hmap.keys() and Hmap[index] != i:
                return[i+1,Hmap[index]+1]
        return None

```

#### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

```java
/**
     * 可以满足奇数位于偶数前面的算法，但是奇数和奇数、偶数和偶数的相对位置不能保证。
     * 时间复杂度O(N)，空间O(1)
     * @param arr
     */
    private static void reOrderArray(int[] arr){
        if(arr==null||arr.length<2)
            return ;
        int left = 0;
        int right = arr.length-1;
        while(left<right){
            while((arr[left]&1)==1){
                left++;
            } 
            while((arr[right]&1)==0){
                right--;
            }
            // 如果不加此处的if判断语句，会导致right已经在left前面了，但是依然进行了交换。
            // 即将已经在前面的奇数和后面的偶数进行了置换！！！
            if(left<right){
                int temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
            }
        }
    }
```

#### [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

摩尔投票法：让异族数1V1去消耗最多数，最终剩下的肯定是最多数

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        score = 0
        for i in range(len(nums)):
            if score == 0: res = nums[i]
            score = score+1 if nums[i] == res else score-1
        return res
```

#### [删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

数组有序,从左向右逐一遍历，只要没遇到过就放到头部（left++以便留出位置），最后返回`nums[:left]`

```python
  class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow = 0
        for i in range(1,len(nums)):
            if nums[i] != nums[slow]:
                slow += 1
                nums[slow] = nums[i]
        nums = nums[:slow]
        return slow+1
```



#### [数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内,所以可以用一个萝卜一个坑的思路，让索引和数一一对应起来，若被占了，说明重复

```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        for i in range(len(nums)):
            #这一步的判断很关键，防止本来人家位置就坐的对着
            if nums[i] == i:continue
            elif nums[nums[i]] == nums[i]:
                return nums[i]
            nums[nums[i]],nums[i] = nums[i],nums[nums[i]]
```

#### [容器盛水问题](https://www.nowcoder.com/practice/31c1aed01b394f0b8b7734de0324e00f?tpId=117&&tqId=37802&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

**核心**：每一个位置能盛最多水等于`min(lmax,rmax)-arr[i]`

```python
class Solution:
    def maxWater(self , arr ):
        res,size = 0, len(arr)
        l,r = 0,size-1
        lmax,rmax = arr[l],arr[r]
        while(l<r):
            lmax = max(lmax,arr[l])
            rmax = max(rmax,arr[r])
            if lmax<=rmax:
                res += lmax - arr[l]
                l += 1
            else:
                res += rmax -arr[r]
                r -= 1
        return res
```

#### [螺旋矩阵(顺时针打印矩阵)](https://www.nowcoder.com/practice/7edf70f2d29c4b599693dc3aaeea1d31?tpId=117&&tqId=37738&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

循环遍历并且用哨兵看着

| 打印方向 | 1. 根据边界打印        | 2. 边界向内收缩  | 3. 是否打印完毕 |
| -------- | ---------------------- | ---------------- | --------------- |
| 从左向右 | 左边界`l` ，右边界 `r` | 上边界 `t` 加 11 | 是否 `t > b`    |
| 从上向下 | 上边界 `t` ，下边界`b` | 右边界 `r` 减 11 | 是否 `l > r`    |
| 从右向左 | 右边界 `r` ，左边界`l` | 下边界 `b` 减 11 | 是否 `t > b`    |
| 从下向上 | 下边界 `b` ，上边界`t` | 左边界 `l` 加 11 | 是否 `l > r`    |

```python
class Solution:
    def spiralOrder(self , matrix ):
        if not matrix:return[]
        l,r,t,b,res = 0,len(matrix[0])-1,0,len(matrix)-1,[]
        while l<=r and t <= b:
            for i in range(l,r+1):res.append(matrix[t][i])
            t += 1
            # 这里需要判断是否结束
            if t > b: break
            for i in range(t,b+1):res.append(matrix[i][r])
            r -= 1
            if l>r: break
            for i in range(r,l-1,-1):res.append(matrix[b][i])
            b -= 1
            if t > b: break
            for i in range(b,t-1,-1):res.append(matrix[i][l])
            l += 1
            if l>r: break
        return res
```

#### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

![Picture1.png](https://pic.leetcode-cn.com/8fec91e89a69d8695be2974de14b74905fcd60393921492bbe0338b0a628fd9a-Picture1.png)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if len(nums) == 1:return nums[0]
        dp = [nums[0]]*len(nums)
        for i in range(1,len(nums)):
            if dp[i-1] > 0: dp[i] = dp[i-1]+nums[i]
            else:dp[i] = nums[i]
        return max(dp)
```

## 链表相关算法考察：

**链表考察点：**链表的操作主要就是对指针的操作，常见面试题目都在考察指针的操作是否合理与正确。

**链表常见算法题：**

#### [单链表反转](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=117&&tqId=37777&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

```java
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        pre = None
        cur = pHead
        while(cur):
            tem = cur.next
            cur.next = pre
            pre = cur
            cur = tem
        return pre
```

#### [k个一组进行反转](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

```java
ListNode reverseKGroup(ListNode head, int k) {
    if (head == null) return null;
    // 区间 [a, b) 包含 k 个待反转元素
    ListNode a, b;
    a = b = head;
    for (int i = 0; i < k; i++) {
        // 不足 k 个，不需要反转，base case
        if (b == null) return head;
        b = b.next;
    }
    // 反转前 k 个元素
    ListNode newHead = reverse(a, b);
    // 递归反转后续链表并连接起来
    a.next = reverseKGroup(b, k);
    return newHead;
}

/** 反转区间 [a, b) 的元素，注意是左闭右开 */
ListNode reverse(ListNode a, ListNode b) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    // while 终止的条件改一下就行了
    while (cur != b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点
    return pre;
}
```

#### 合并有序单链表



#### 找出单链表的中间节点



#### [判断链表有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=117&&tqId=37714&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

```python
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        # 这里的判断比较关键
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(fast == slow) return true;
        }
        return false;
    }
}
```

#### [找出进入环的第一个节点](https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=117&&tqId=37713&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

快指针比慢指针多走了一个环，而路程是是慢指针的两倍，所以慢指针路程==环长度，假设相遇点距离起点为m,那么head距离入口也为m

```python
class Solution:
    def detectCycle(self , head ):
        slow,fast = head,head
        while(fast != None and fast.next != None):
            slow = slow.next
            fast = fast.next.next
            if fast == slow: break
        # 防止没有环
        if fast == None or fast.next == None: return None
        slow = head
        while(fast != slow):
            slow = slow.next
            fast = fast.next
        return slow
```

#### [求单链表相交的第一个节点](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        A, B = headA, headB
        while A != B:
            A = A.next if A else headB
            B = B.next if B else headA
        return A
```

#### [两个链表生成相加链表](https://www.nowcoder.com/practice/c56f6c70fb3f4849bc56e33ff2a50b6b?tpId=117&&tqId=37814&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

```c++
    stack<int> st1;//存储链表1的数据
    stack<int> st2;//存储链表2的数据

    while(head1||head2)//head1和head2都遍历完时推出循环
    {
        if(head1)
        {
        st1.push(head1->val);
        head1=head1->next;
        }
        if(head2)
        {
        st2.push(head2->val);
        head2=head2->next; 
        }
    }
    ListNode *vhead=new ListNode(-1);
    int carry=0;
    while(!st1.empty()||!st2.empty()||carry!=0)//这里设置carry!=0,是因为当st1,st2都遍历完时，如果carry=0,就不需要进入循环了
    {
        int a=0,b=0;
        if(!st1.empty())
        {
            a=st1.top();
            st1.pop();
        }
        if(!st2.empty())
        {
            b=st2.top();
            st2.pop();
        }
        int get_sum=a+b+carry;//每次的和应该是对应位相加再加上进位
        int ans=get_sum%10;//对累加的结果取余
        carry=get_sum/10;//如果大于0，就进位
        ListNode *cur=new ListNode(ans);//创建节点
        cur->next=vhead->next;
        vhead->next=cur;//每次把最新得到的节点更新到vhead->next中
    }
    return vhead->next;
```

#### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

```python
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head: return
        cur = head
        # 1. 复制各节点，并构建拼接链表
        while cur:
            tmp = Node(cur.val)
            tmp.next = cur.next
            cur.next = tmp
            cur = tmp.next
        # 2. 构建各新节点的 random 指向
        cur = head
        while cur:
            if cur.random:
                cur.next.random = cur.random.next
            cur = cur.next.next
        # 3. 拆分两链表
        cur = res = head.next
        pre = head
        while cur.next:
            pre.next = pre.next.next
            cur.next = cur.next.next
            pre = pre.next
            cur = cur.next
        pre.next = None # 单独处理原链表尾节点
        return res      # 返回新链表头节点
```



## 二叉树相关算法考察：

**二叉树常见算法题：**

#### [分层遍历（宽度优先遍历）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root: return []
        res, queue = [], collections.deque()
        queue.append(root)
        while queue:
            tmp = []
            for _ in range(len(queue)):
                node = queue.popleft()
                tmp.append(node.val)
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(tmp[::-1] if len(res) % 2 else tmp)
        return res
```

#### 前序遍历，中序遍历，后序遍历

递归写法太简单，以下是非递归写法

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/liaT5dytkaTfrDKKBt1SMHeJL6qDia4CLbSayGer6vIH5axVSiciawBfDI7CyYuiaUN1tofQrFBtFxDppAH6PO8UboQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### [求二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

```java
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root: return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

#### 判断两颗二叉树是否是相同的树

太简单了不想写

#### [求二叉树中两个节点的最低公共祖先节点](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // base case
        if(root == null) return null;
        if(root == p || root == q) return root;
        // 递归调用
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        // 分情况讨论
        if(left != null && right != null) return root;
        if(left == null && right == null) return null;
        return left != null?left:right;
    }
}
```

#### [重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        return self.recur(preorder,0,len(preorder)-1,inorder,0,len(inorder)-1)
    def recur(self,preorder,pl,pr,inorder,il,ir):
        hmap = dict()
        for i,val in enumerate(inorder):
            hmap[val] = i
            # base case 两个边界都可以用来判断
        if il > ir or pl > pr:return
        root = TreeNode(preorder[pl])
        index = hmap[preorder[pl]]
        leftsize = index-il
        # 前序遍历的左侧边界不要只加了leftsize而忘了加上pl
        root.left = self.recur(preorder,pl+1,pl+leftsize,inorder,il,index-1)
        root.right = self.recur(preorder,pl+leftsize+1,pr,inorder,index+1,ir)
        return root
```

#### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null || B == null) return false;
        return recur(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }

    public boolean recur(TreeNode A, TreeNode B){
        if(B == null) return true;
        if(A == null) return false;
        return (A.val == B.val) && recur(A.left,B.left) && recur(A.right,B.right);
    }
}
```

#### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

```python
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        res, path = [], []
        def recur(root, tar):
            # 这一步也不能省略，这是在其他情况下的返回，不然会报 root NoneType的错
            if not root: return
            path.append(root.val)
            tar -= root.val
            if tar == 0 and not root.left and not root.right:
                # 值得注意的是，记录路径时若直接执行 res.append(path) ，则是将 path 对象加入了 res ；后续 path 改变时， res 中的 path 对象也会随				# 之改变。
                # 正确做法：res.append(list(path)) ，相当于复制了一个 path 并加入到 res 
                res.append(list(path))
            recur(root.left, tar)
            recur(root.right, tar)
           # 回溯，清理现场
            path.pop()
            
        recur(root, sum)
        return res
```

#### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

思路：数组最后一个数为root，小于root的为左子树，大于root的为右子树，对于左右子树的序列亦是如此。

解法1：递归判断，满足条件才返回true

```python
class Solution:
    def verifyPostorder(self, postorder: List[int]) -> bool:
        def recur(i,j):
            # base case: 必须要有
            if i >= j: return True
            p = i 
            while postorder[p] < postorder[j]: p += 1
            m = p 
            while postorder[p] > postorder[j]: p += 1
            # 注意：m是大于arr[j]的，因此右子树是从m到 *j-1*
            return p == j and recur(i,m-1) and recur(m,j-1)  
        return recur(0,len(postorder)-1)
```

解法2：[辅助单调栈](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/solution/mian-shi-ti-33-er-cha-sou-suo-shu-de-hou-xu-bian-6/)，还没看

#### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

**算法流程：**

**`dfs(cur):`** 递归法中序遍历；

1. **终止条件：** 当节点 `cur` 为空，代表越过叶节点，直接返回；
2. 递归左子树，即 `dfs(cur.left)` ；
3. 构建链表：
   1. **当 `pre` 为空时：** 代表正在访问链表头节点，记为 `head` ；
   2. **当 `pre` 不为空时：** 修改双向节点引用，即 `pre.right = cur` ， `cur.left = pre` ；
   3. **保存 `cur` ：** 更新 `pre = cur` ，即节点 `cur` 是后继节点的 `pre` ；
4. 递归右子树，即 `dfs(cur.right)` ；

```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        def dfs(cur):
            if not cur: return
            dfs(cur.left) # 递归左子树
            if self.pre: # 修改节点引用
                self.pre.right, cur.left = cur, self.pre
            else: # 记录头节点
                self.head = cur
            self.pre = cur # 保存 cur
            dfs(cur.right) # 递归右子树
        
        if not root: return
        self.pre = None
        dfs(root)
        self.head.left, self.pre.right = self.pre, self.head
        return self.head
```

[剑指 Offer 37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)



## 队列与堆栈：

在算法题目的考察中，不光将队列和堆栈做为辅助数据结构考察，还会直接对其相关特性进行考察，典型的算法题目如下：

#### [包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

**算法思路：**

题目要求我们的各个方法均为O(1)复杂度，我们考虑增加辅助空间来实现，即增加一个专门用来存储min值的辅助栈。

**实现步骤：**

- 比如，data依次入栈：5, 4, 3, 8, 10, 11, 12, 1。则辅助栈依次入栈：5, 4, 3，no，no, no, no, 1。其中，no代表此次不入栈。也就是说每次入栈的时候，如果入栈的元素比min中的栈顶元素小或等于则入栈，否则不入栈。
- 当出栈的时候，我们比较辅助栈与当前出栈的值是否相等。如果相等，则辅助栈栈顶元素也需要出栈。
- 当需要获取栈中最小元素的时候，我们直接获取到辅助栈的栈顶元素即可。
- 这里需要注意的是，在获取栈中的min值时，我们应该使用minStack.peek方法而不是minStack.pop方法。peek方法仅仅是获取数值，但是pop方法则会执行出栈操作。

```python
class MinStack:
    def __init__(self):
        self.A, self.B = [], []

    def push(self, x: int) -> None:
        self.A.append(x)
        # 此处必须是小于等于
        if not self.B or self.B[-1] >= x:
            self.B.append(x)

    def pop(self) -> None:
        if self.A.pop() == self.B[-1]:
            self.B.pop()

    def top(self) -> int:
        return self.A[-1]

    def min(self) -> int:
        return self.B[-1]
```

#### [用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=117&&tqId=37774&rp=1&ru=/ta/job-code-high&qru=/ta/job-code-high/question-ranking)

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stake1 = []
        self.stake2 = []
    
    def push(self, node):
        self.stake1.append(node)
        
    def pop(self):
#         必须2不空1才出栈，这一步很关键
        if(not self.stake2):
            while(self.stake1):
                self.stake2.append(self.stake1.pop())
        if(self.stake2): return self.stake2.pop()
        else: return -1python
```

#### [用两个队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

```python
class MyStack:

    def __init__(self):
        self.queue1 = collections.deque()
        self.queue2 = collections.deque()

    def push(self, x: int) -> None:
        #有了新元素，先放到辅助队列，再将主队列依次放入辅助队列并保持主副关系，实现后进先出
        self.queue2.append(x)
        while self.queue1:
            self.queue2.append(self.queue1.popleft())
        self.queue1, self.queue2 = self.queue2, self.queue1

    def pop(self) -> int:
        return self.queue1.popleft()
```

#### [LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```python
class ListNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.hashmap = {}
        # 新建两个节点 head 和 tail
        self.head = ListNode()
        self.tail = ListNode()
        # 初始化链表为 head <-> tail
        self.head.next = self.tail
        self.tail.prev = self.head

    # 因为get与put操作都可能需要将双向链表中的某个节点移到末尾，所以定义一个方法
    def move_node_to_tail(self, key):
            # 先将哈希表key指向的节点拎出来，为了简洁起名node
            #      hashmap[key]                               hashmap[key]
            #           |                                          |
            #           V              -->                         V
            # prev <-> node <-> next         pre <-> next   ...   node
            node = self.hashmap[key]
            node.prev.next = node.next
            node.next.prev = node.prev
            # 之后将node插入到尾节点前
            #                 hashmap[key]                 hashmap[key]
            #                      |                            |
            #                      V        -->                 V
            # prev <-> tail  ...  node                prev <-> node <-> tail
            node.prev = self.tail.prev
            node.next = self.tail
            self.tail.prev.next = node
            self.tail.prev = node

    def get(self, key: int) -> int:
        if key in self.hashmap:
            # 如果已经在链表中了久把它移到末尾（变成最新访问的）
            self.move_node_to_tail(key)
        res = self.hashmap.get(key, -1)
        if res == -1:
            return res
        else:
            return res.value

    def put(self, key: int, value: int) -> None:
        if key in self.hashmap:
            # 如果key本身已经在哈希表中了就不需要在链表中加入新的节点
            # 但是需要更新字典该值对应节点的value
            self.hashmap[key].value = value
            # 之后将该节点移到末尾
            self.move_node_to_tail(key)
        else:
            if len(self.hashmap) == self.capacity:
                # 去掉哈希表对应项
                self.hashmap.pop(self.head.next.key)
                # 去掉最久没有被访问过的节点，即头节点之后的节点
                self.head.next = self.head.next.next
                self.head.next.prev = self.head
            # 如果不在的话就插入到尾节点前
            new = ListNode(key, value)
            self.hashmap[key] = new
            new.prev = self.tail.prev
            new.next = self.tail
            self.tail.prev.next = new
            self.tail.prev = new
```

#### [单调队列：滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

滑动窗口是一个从左到右的递减序列

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums:return[]
        queue,res = [],[]
        for i in range(len(nums)):
            if i >= k and i-queue[0] >= k:queue.pop(0)
            while queue and nums[queue[-1]] < nums[i]:
                queue.pop()
            queue.append(i)
            if i >= k-1: res.append(nums[queue[0]])
        return res
```

#### [单调栈：每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

stack 从顶向下越来越大，如果栈顶元素干不过当前温度，那要退出，因为他是夹在中间的矮个子没啥存在的意义，但如果干得过，那栈顶就是第一个大于当前温度的，求结果加入结果集

```python
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        if not temperatures:return []
        stack,res = [],[0]*len(temperatures)
        for i in range(len(temperatures)-1,-1,-1):
            while stack and temperatures[stack[-1]] <= temperatures[i]:
                stack.pop()
            res[i] = stack[-1]-i if stack else 0
            stack.append(i)
        return res
```

> **总结：**其实单调队列和单调栈没啥区别，就是看逆序遍历还是顺序遍历。而**单调队列和优先队列的区别**在于：单调队列是通过实时对比和剔除加入来保持元素的有序，而优先队列是自己调整内部元素来保持有序，是通过红黑树或者斐波那契堆来实现的。

以下是一道不典型的优先队列或者说小根堆来例题

703 数据流的第K大元素

![image-20210611180122505](E:\nutstore\md\javaInterview\README.assets\image-20210611180122505.png)

## 字符串相关算法：

在开始交流字符串面试题目之前，我们先来简单介绍下**子串和子序列的区别**吧。

1. **子串**：字符串中任意个**连续的字符**组成的子序列。
2. **子序列**：字符串中**按照前后顺序**取出的任意个字符组成，**不要求连续**。

#### [有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```python
class Solution:
    def isValid(self , s ):
        dic = {'{': '}',  '[': ']', '(': ')', '?': '?'}
        stack = ['?']
        for c in s:
            if c in dic: stack.append(c)
            elif dic[stack.pop()] != c: return False 
        return len(stack) == 1
```

#### [ 最长不含重复字符的子串（数组）](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

子数组：java解法

```java
public class Solution {
    public int maxLength (int[] arr) {
        HashMap<Integer,Integer> map = new HashMap<>();
        int result = 0;
        for (int left = 0, right = 0; right < arr.length; right++) {
            if(map.containsKey(arr[right])) left = Math.max(left,map.get(arr[right])+1);
            result = Math.max(result, right - left +1);
            map.put(arr[right], right);
        }
        return result;
    }
}
```

子串：python解法（对比学习二者数据结构）

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dic, res, i = {}, 0, -1
        for j in range(len(s)):
            if s[j] in dic:
                i = max(dic[s[j]], i) # 更新左指针 i
            dic[s[j]] = j # 哈希表记录
            res = max(res, j - i) # 更新结果
        return res
```

#### [最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m,n = len(text1), len(text2)
        dp = [(n+1)*[0] for i in range(m+1)]
        for i in range(1,m+1):
            for j in range(1,n+1):
                if text1[i-1] == text2[j-1] :
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
        return dp[m][n]
```

#### [最长公共子串](https://www.nowcoder.com/practice/f33f5adc55f444baa0e0ca87ad8a6aac?tpId=117&rp=1&ru=%2Fta%2Fjob-code-high&qru=%2Fta%2Fjob-code-high%2Fquestion-ranking)

```java
    public String LCS (String str1, String str2) {
        int m = str1.length(), n = str2.length();
        int[][] dp = new int[m + 1][n + 1];
        int max = 0, index = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (str1.charAt(i) == str2.charAt(j)) {
                    dp[i + 1][j + 1] = dp[i][j] + 1;
                    if (max < dp[i + 1][j + 1]) {
                        max = dp[i + 1][j + 1];
                        index = i + 1;
                    }
                }
            }
        }
        return max == 0 ? "-1" : str1.substring(index-max,index);
    }
}
```

#### [最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for i in range(1,len(nums)):
            for j in range(0,i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i],dp[j]+1)
        return max(dp)
```

#### [最长回文子串](https://www.nowcoder.com/practice/b4525d1d84934cf280439aeecc36f4af?tpId=117&tags=&title=&difficulty=0&judgeStatus=0&rp=0)

```python
class Solution:
    def getLongestPalindrome(self, A, n):
        # 求最长回文子串的方法,引入两个边界是针对abba,aba类似的这两类回文子串的判断
        def Palindrome(A,l,r):
            while l >= 0 and r<len(A) and A[l] == A[r]:
                l -= 1
                r += 1
            # 数组切片是左闭右开
            return A[l+1:r]
        res = 0
        for i in range(len(A)):
            l1 = Palindrome(A, i, i)
            l2 = Palindrome(A, i, i+1)
            res = max(res,len(l1),len(l2))
        return res
```

[最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)



[剑指 Offer 67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)



## 动态规划

#### 青蛙跳台阶问题。

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级台阶总共有多少种跳法？

**分析：**

当n = 1， 只有1中跳法；当n = 2时，有2种跳法；当n = 3 时，有3种跳法；当n = 4时，有5种跳法；当n = 5时，有8种跳法；.......规律类似于Fibonacci数列：

![图片说明](https://uploadfiles.nowcoder.com/images/20191126/5459305_1574726608577_F3E7B4A76AD61143F52E38396E259B80)

所以，我们马上可以写出如下的递归实现代码：

```
public int Fibonacci(int n){  
    if(n<=2)  
        return n;  
    return Fibonacci(n-1)+Fibonacci(n-2);  
}  
```

当我们写出递归代码时，面试官应该会建议我们对递归代码进行优化，因为递归代码中有太多的重复运算。所以，我们考虑使用使用变量保存住中间结果。 代码实现如下：

```java
public int jumpFloor(int number) {  
    if(number<=2)  
        return number;  
    int jumpone=2; // 离所求的number的距离为1步的情况，有多少种跳法  
    int jumptwo=1; // 离所求的number的距离为2步的情况，有多少种跳法  
    int sum=0;  
    for(int i=3;i<=number;i++){  
        sum=jumptwo+jumpone;  
        jumptwo=jumpone;  
        jumpone=sum;  
    }  
    return sum;  
}
```

接下来，我们继续看青蛙跳台阶的变态版。

**题目二：青蛙变态跳台阶问题**

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

**思路：**

- 先跳到n-1级，再一步跳到n级，有f(n-1)种；
- 先跳到n-2级，再一步跳到n级，有f(n-2)种；
- 先跳到n-3级，再一步跳到n级，有f(n-3)种；
- ……
- 先跳到第1级，再一步跳到n级，有f(1)种；



所以，可以推出如下的公式：

- f(n)=f(n-1)+f(n-2)+f(n-3)+•••+f(1)
- f(n-1)=f(n-2)+f(n-3)+•••+f(1)
- 推出f(n)=2*f(n-1)

**算法实现如下：**

```java
public int jumpFloor2(int num) {  
    if(num<=2)  
        return num;  
    int jumpone=2; // 前面一级台阶的总跳法数  
    int sum=0;  
    for(int i=3;i<=num;i++){  
        sum = 2*jumpone;  
        jumpone = sum;  
    }  
    return sum;  
}
```

#### [股票买卖](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

最初版本：遍历时记录下最低成本，如果新的卖出价获益更高则更新，否则保持记录

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0: return 0
        dp = [0]*len(prices)
        cost = prices[0]
        for i in range (1,len(prices)):
            cost = min(cost,prices[i])
            dp[i] = max(dp[i-1],prices[i]-cost) 
        return max(dp)
```

优化空间复杂度：利润只与前一天利润和当前成本有关，所以利润只保留最大值

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0: return 0
        profit = 0
        cost = prices[0]
        for i in range (1,len(prices)):
            cost = min(cost,prices[i])
            profit = max(profit,prices[i]-cost) 
        return profit
```

## 回溯算法

回溯算法框架：

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

#### [排列（字符不重复）](https://leetcode-cn.com/problems/permutations/)

做选择时如果新加入的数已经在选择列表里面了则返回

![image.png](https://pic.leetcode-cn.com/0bf18f9b86a2542d1f6aa8db6cc45475fce5aa329a07ca02a9357c2ead81eec1-image.png)

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        path,res = [],[]
        def recur(nums,path):
            if len(path) == len(nums):
                res.append(list(path))
            for i in nums:
                if i in path:continue
                path.append(i)
                recur(nums,path)
                path.pop()
        recur(nums,path)
        return res
```

#### [排列（字符可重复）](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

![image-20210612122137232](../images/image-20210612122137232.png)

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        # 先排序才能用后面的判断
        nums.sort()
        path,res = [],[]
        def recur(nums,path,used):
            if len(path) == len(nums):
                res.append(path[:])
            for i in range(size):
                if not used[i]:
                    # 这一步的判断着实很关键
                    if i>0 and nums[i] == nums[i-1] and  used[i-1]:continue
                    used[i] = True
                    path.append(nums[i])
                    recur(nums,path,used)
                    used[i] = False
                    path.pop()
        size = len(nums)
        used = [False]*size
        recur(nums,path,used)
        return res
```

#### [组合](https://leetcode-cn.com/problems/combinations/)

**核心**：用`i`去控制选择范围![image.png](https://pic.leetcode-cn.com/1599488203-TzmCXb-image.png)

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        path = [] 
        def recur(n,path,start):
            if len(path) == k:
                # 这里应该用深拷贝，如果只写path，path会随着后面的引用而改变
                res.append(path[:])
            for j in range(start,n+1):
                path.append(j)
                # 递归开始点为j+1而不是start+1
                recur(n,path,j+1)
                path.pop()
        recur(n,path,1)
        return res 
```

N皇后问题



## 设计模式

#### 单例模式

```java
public class Single {
    public static void main(String[] args) {

    }
}

/**
 *饿汉式的单例模式
 * @author ywq
 */
class Single1{
    private Single1(){}
    private static final Single s = new Single();
    public static Single getInstance(){
        return s;
    }
}

/**
 *懒汉式的单例模式
 * @author ywq
 */
class Single2{
    private Single2(){}
    private static Single s = null;
    public static  Single getInstance(){
        if(null==s)
            s = new Single();
        return s;
    }
}

/**
 * 多线程环境下的懒汉式单例模式(DCL，双检锁实现)
 * @author ywq
 */
class Single3{
    private static Single s = null;
    private Single3(){}

    public static  Single getInstance(){
        if(null==s){
            synchronized(Single.class){
                if(null==s)
                /**
                 * 1. 分配内存空间
                 * 2. 执行构造方法，初始化对象
                 * 3. 把对象指向这个空间
                 */
                    s = new Single();
            }
        }
        return s;
    }
}


/**
 * 多线程环境下的懒汉式单例模式(DCL，双检锁+volatile实现)
 * 加入了volatile变量来禁止指令重排序
 * @author ywq
 */
class Single4{
    private Single4(){}
    private static volatile Single s = null;

    public static  Single getInstance(){
        if(null==s){
            synchronized(Single.class){
                if(null==s)
                    s = new Single();
            }
        }
        return s;
    }
}
```

#### 多线程设计

```java
package niuke.thread;

public class WindowTicket {

    public static void main(String[] args) {

        TicketSale ticketSale = new TicketSale();
        Thread Sale1 = new Thread(ticketSale, "售票口1");
        Thread Sale2 = new Thread(ticketSale, "售票口2");
        Thread Sale3 = new Thread(ticketSale, "售票口3");
        Thread Sale4 = new Thread(ticketSale, "售票口4");
        // 启动线程，开始售票
        Sale1.start();
        Sale2.start();
        Sale3.start();
        Sale4.start();
    }
}

class TicketSale implements Runnable {
    int ticketSum = 100;

    @Override
    public void run() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 有余票，就卖
        while (ticketSum > 0) {
            System.out.println(Thread.currentThread().getName() + "售出第" + (100 - ticketSum + 1) + "张票");
            ticketSum--;
        }
        System.out.println(Thread.currentThread().getName() + "表示没有票了");
    }
}

```

代理模式

## 系统设计

#### 扫码登录



#### 抢红包



#### 短网址



## 位运算

#### 不用额外变量交换两个数

```java
a = a^b;
b = a^b;
a = a^b;
```

#### [剑指 Offer 65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

```python
class Solution:
    def add(self, a: int, b: int) -> int:
        x = 0xffffffff
        a, b = a & x, b & x
        while b != 0:
            a, b = (a ^ b), (a & b) << 1 & x
        return a if a <= 0x7fffffff else ~(a ^ x)
```



#### 数组中出现了奇数次的数



#### 数组中有两个数出现了奇数次

 

## [大数据](https://mp.weixin.qq.com/s?__biz=MzU1NTA0NTEwMg==&mid=2247483860&idx=1&sn=e211f83b5fea6abc87c724579a28a883&chksm=fbdb1855ccac914371ba9f7da9db2f072964c1eada5176312a00eb7d19522954a0bff0f0e1f7&scene=21#wechat_redirect)

关键点：分而治之，常用hashmap和bitmap

#### 10 亿个IP进行排序

#### 10 亿个年龄排序

#### 2G内存在20亿个32位大整数中找到出现次数最多的数

哈希需要20亿*8字节=16G，因此进行哈希分流到8个机器

#### 10M内存找到40亿个数中没有的自然数

bitmap需要500m内存，因此分区间，2^32/64,找到不满的区间再进行bitmap填充

#### 百亿单次找到最热的100词

分流，堆，外部排序

#### 一致性哈希算法

![把一致性哈希算法原理讲的最清楚的一篇- 知乎](https://pic4.zhimg.com/v2-2f6bd808d11171fa3ae9accf6927e107_b.jpg)

**布隆过滤器** 特点： 网页黑名单系统，垃圾邮件过滤系统，爬虫的网址判断重复系统，容忍一定程度的失误率，对空间要求较严格

![img](https://user-gold-cdn.xitu.io/2019/11/30/16eba60985ae27ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## [智力题](https://mp.weixin.qq.com/s?__biz=MzU1NTA0NTEwMg==&mid=2247483985&idx=1&sn=5cd8587506ea5b6dd7bebfbb00db94ed&chksm=fbdb1bd0ccac92c608fd8cc41f4500ff0a385b6c8c21a6ada45de5570669ce18d6414540c011&scene=21#wechat_redirect)

#### 狼吃羊

狼为奇数：吃

狼为偶数：不吃

#### 烧绳子



#### 倒水

> **不断用小桶装水倒入大桶，大桶满了立即清空，每次判断下二个桶中水的容量是否等于指定容量。**



