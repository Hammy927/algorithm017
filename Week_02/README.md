[toc]

# 1.数组

## 1.1 加一(2020-09-23 第二遍,2020-09-30 第三遍)

### 1.1.1 题目描述

给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。


示例 1:

```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。

```

示例 2:

```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。

```

### 1.1.2 基本思路

从尾到头遍历数组，如果数组中的尾巴不为9，那么直接+1，结束遍历。

如果尾巴为9的话，那么当前队尾的值更新为0，前置值进行+1操作，如果前置为9依次类推，直至遍历完成。


### 1.1.3 代码实现

#### 1.1.3.1 解法1(时间复杂度O(n),空间复杂度O(n))

```

class Solution {
    public int[] plusOne(int[] digits) {
        for(int i = digits.length - 1; i >= 0; i--){
            digits[i]++;
            digits[i] = digits[i] % 10 ;
            if(digits[i] != 0){
                return digits;
            }
        }
        //如果走到这个部分，说明之前第一位也进位了，需要对数组进行扩容
        digits = new int[digits.length + 1];
        digits[0] = 1;
        //当前情况，除了第一位是1以外，其他位都是零
        return digits;
    }
}


```

## 1.2  移动零(2020-9-24 第二遍,2020-09-30=>第三遍)

### 1.2.1 题目描述 

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

说明:

1.必须在原数组上操作，不能拷贝额外的数组。

2.尽量减少操作次数。

### 1.2.2 基本思路

双指针，i指针用来遍历数组，j指针用来记录非0元素的位置。将i遍历完成后，当前j指向是最后一个非0元素，那么剩余的元素就全部为0了.

解法1的时间复杂度虽然是O（1），但是j会一直赋值i为非0的操作，这个操作其实是可以优化的


### 1.2.3 代码实现

#### 1.2.3.1 解法1(时间复杂度O(n),空间复杂度O(1))

```
class Solution {
    public void moveZeroes(int[] nums) {
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[j++] = nums[i];
            }
        }

        while (j < nums.length) {
            nums[j++] = 0;
        }
    }
}

```

#### 1.2.3.2 解法2(时间复杂度O(n),空间复杂度O(1))

```
   int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                //当前j指向的0的位置，i > j说明i指向了非0元素，而j当前的元素为0
                if (i > j) {
                    nums[j] = nums[i];
                    nums[i] = 0;
                }

                j++;
            }
        }

```





## 1.3 两数之和(2020-09-24 第二遍,2020-09-30=>第三遍)

### 1.3.1 题目描述

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍.

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

```

### 1.3.2 基本思路

两种思路，第一种就是暴力求解法，两层循环，第一层是遍历num数组，第二层是遍历当前num数组中元素相加等于目标和的值。

第二种思路是使用map将遍历将相加等于目标和的值进行存储，如果遍历过程中，有出现
等于目标和的值那么结束判断，否则加入到map中.

### 1.3.3 代码内容

#### 1.3.3.1 解法一(时间复杂度O(n^2),空间复杂度O(1)):

```
class Solution {
   public int[] twoSum(int[] nums, int target) {
        for(int i = 0; i < nums.length - 1; i++){
            int other = target - nums[i];
            for(int j = i + 1; j < nums.length;j++){
                if(nums[j] == other){
                    return new int[]{i,j};
                }
            }
        }

        return new int[]{};
    }
}

```

#### 1.3.3.1 解法二(时间复杂度O(n),空间复杂度O(n)):

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap(nums.length);
        for(int i = 0; i < nums.length; i++){
            int other = target - nums[i];
            if(map.containsKey(other)){
                int otherIndex = map.get(other);
                return new int[]{i,otherIndex};
            }

            map.put(nums[i],i);
        }

        return new int[]{};
    }
}
```

## 1.4 盛水最多的容器(2020-09-25第一遍,2020-09-27=>第二遍)

### 1.4.1 题目描述

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, 
//ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。 

### 1.4.2 基本思路

//todo

### 1.4.3 代码内容

#### 1.4.3.1 解法1，时间复杂度(O(n^2)),空间复杂度O(1))

```
class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        for (int i = 0; i < height.length - 1; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int area = (j - i) * Math.min(height[i], height[j]);
                if (area > max) {
                    max = area;
                }
            }
        }

        return max;
    }
}
```


#### 1.4.3.2 解法2，时间复杂度(O(n),空间复杂度O(1))

```
  public int maxArea(int[] height) {
        int max = 0;
        int i = 0;
        int j = height.length - 1;
        while (i != j) {
            int heigit = height[i] > height[j] ? height[j--] : height[i++];
            //+1是因为上面已经--了
            int area = (j - i  + 1) * heigit;
            max = Math.max(area,max);
        }

        return max;
    }

```

## 1.5 三数之和(2020-09-27第一遍)

### 1.5.1 题目描述

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]


### 1.5.2 基本思路

### 1.5.3 代码实现

#### 1.5.3.1 解法1:时间复杂度O(n^3),空间复杂度O(n)

```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> set = new HashSet<>();
        //第一种暴力求解法,保证顺序一致性
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            for (int j = i + 1; j < nums.length - 1; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> res = Arrays.asList(nums[i], nums[j], nums[k]);
                        set.add(res);
                    }
                }
            }
        }
        
        return new ArrayList<>(set);
    }
}
```

#### 1.5.3.2 解法2:时间复杂度O(n^2),空间复杂度O(1)

```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> res = new ArrayList<>();
        //只有排序以后才能使用双指针法
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            //因为数组是有序的，后序的数字不能再出现上述情况
            if (nums[i] > 0) {
                break;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int k = i + 1;
            int j = nums.length - 1;
            while (k < j) {
                int sum = nums[i] + nums[k] + nums[j];
                if (sum < 0) {
                    while (k < j && nums[k] == nums[++k]);
                } else if (sum > 0) {
                    while (j > k && nums[j] == nums[--j]);
                } else {
                    res.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    while (k < j && nums[k] == nums[++k]);
                    while (j > k && nums[j] == nums[--j]);
                }
            }

        }

        return res;
    }
}


```

## 1.6 删除重复数组中的元素(2020-09-27日第一遍)

### 1.6.1 题目描述

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

### 1.6.2 基本思路

//todo 

### 1.6.3 代码内容

#### 1.6.3.1 解法1,时间复杂度O(n),空间复杂度O(1)

```
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int i = 0;
        for (int j = 1; j < nums.length; j++) {
            if (nums[j] != nums[i]) {
                if (j - i > 1) {
                    nums[i + 1] = nums[j];
                }

                i++;
            }
        }

        return i + 1;
    }
}
```

## 1.7 柱状图最大的矩形

### 1.7.1 题目描述

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

![image](http://note.youdao.com/yws/res/45381/038E589E7E2B43C182E217049BF6FDEA)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

![image](http://note.youdao.com/yws/res/45380/3D731BD03E9B4D2EBF103D0C854D71BE)

示例:

```
输入: [2,1,5,6,2,3]
输出: 10
```

### 1.7.2 基本思路

### 1.7.3 代码内容

#### 1.7.3.1 解法1,时间复杂度O(n),空间复杂度O(1)

```
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int res = 0;
        for (int i = 0; i < n; i++) {
            int minHeight = Integer.MAX_VALUE;
            for (int j = i; j < n; j++) {
                minHeight = Math.min(minHeight,heights[j]);
                int area = (j - i + 1) * minHeight;
                if (area > res) {
                    res = area;
                }
            }
        }
        return res;
    }
}

```

## 1.8 有效字母的异位符

### 1.8.1 题目描述

### 1.8.2 基本思路

### 1.8.3 代码内容

#### 1.8.3.1 解法1:时间复杂度O(1),空间复杂度O(nlogn)

```
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s == null || t == null || t.length() != s.length()) {
            return false;
        }

        char[] chars = s.toCharArray();
        char[] others = t.toCharArray();

        Arrays.sort(chars);
        Arrays.sort(others);
        for (int i = 0; i < others.length; i++) {
            if (chars[i] != others[i]) {
                return false;
            }
        }

        return true;
     
    }
}
```

#### 1.8.3.2 解法2:时间复杂度O(n),空间复杂度O(1)

```
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        int[] counter = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counter[s.charAt(i) - 'a']++;
            counter[t.charAt(i) - 'a']--;
        }

        for (int count : counter) {
            if (count != 0) {
                return false;
            }
        }

        return true;
    }
}

```

## 1.9 字母异位词分组(2020-10-04第二遍)

### 1.9.1 题目描述

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

### 1.9.2 基本思路

### 1.9.3 代码内容

#### 1.9.3.1 解法1:时间复杂度O(n),空间复杂度O(n)

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return new ArrayList<>();
        }

        List<List<String>> res = new ArrayList<>();
        Map<String, List<String>> map = new HashMap<>();
        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);

            String ele = String.valueOf(chars);
            if (!map.containsKey(ele)) {
                map.put(ele, new ArrayList<>());
            }

            map.get(ele).add(str);
        }

        return new ArrayList<>(map.values());
    }
}
```

#### 1.9.3.2 解法2:时间复杂度O(n),空间复杂度O(n)

```
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null  || strs.length == 0) {
            return new ArrayList<>();
        }

        Map<String,List<String>> map = new HashMap<>();
        for (String str : strs) {
            char[] charArr = new char[26];
            for (char ch:str.toCharArray()) {
                charArr[ch - 'a']++;
            }

            String newStr = String.valueOf(charArr);
            if (!map.containsKey(newStr)) {
                map.put(newStr,new ArrayList<>());
            }

            map.get(newStr).add(str);
        }

        return new ArrayList(map.values());
    }
}
```


# 2.递归

## 2.1 爬楼梯(2020-09-22 第二遍,2020-09-29=>第三遍)

### 2.1.1 题目描述

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

```

示例 2：

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

### 2.1.2 基本思路

- 题目中的条件为正整数，代码中不需要对输入条件做判断
- 通过计算可得，f(1)=1,f(2)=2,f(3)=3,f(4)=5...根据规律，可得f(n)=f(n-1)+f(n-2)
- 根据这个定式，我们可以用递归来做，但是递归会存在一些冗余的操作，所以可以通过迭代进将历史计算的计算结果保存。

### 2.2.3 代码内容

#### 2.2.3.1 解法一:递归，时间复杂度O(2^n)，空间复杂度O(1)

```
class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
        }
        
        return climbStairs(n-1) + climbStairs(n-2);
    }
}
```

![image](http://note.youdao.com/yws/res/43073/F778D7D218CB486F8DFAE7942F994D73)

![image](http://note.youdao.com/yws/res/43075/579EBCAD70FB40FD8A2B66ACCE205BA0)

#### 2.2.3.2 解法二:迭代,时间复杂度O(n)，空间复杂度O(1)

```
class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
        }
        
        int res = 0;
        int fir = 1;
        int sec = 2;
        for (int i = 2;i < n;i++) {
            res = fir + sec;
            fir = sec;
            sec = res;
        }
        
        return res;
    }
}

```

## 7.1 括号生成

### 7.1.1 题目描述

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]

```

### 7.1.2 基本思路

### 7.1.3 代码内容

#### 7.1.3.1 递归法，时间复杂度未知，空间复杂度未知

```
class Solution {

    private List<String> res = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        this.generator(0,0,n,"");
        return res;
    }

    private void generator(int left,int right,int n,String s){
        if (left == n && right == n) {
            res.add(s);
            return;
        }

        if (left <= n) {
            this.generator(left + 1,right,n,s + "(");
        }

        if (left > right) {
            this.generator(left,right + 1,n,s + ")");
        }
    }
}

```

## 7.2 验证二叉搜索树

### 7.2.1 题目描述

### 7.2.2 基本思路

### 7.2.3 代码实现

#### 2.3.3.1 解法1:递归时间，时间复杂度O(n),空间复杂度O(n)

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
        public boolean isValidBST(TreeNode root) {

        return this.helper(root, null, null);
    }

    private boolean helper(TreeNode node, Integer lower, Integer upper) {
        if (node == null) {
            return true;
        }

        int val = node.val;
        if (lower != null && val <= lower) {
            return false;
        }
        if (upper != null && val >= upper) {
            return false;
        }

        if (!helper(node.right, val, upper)) {
            return false;
        }

        if (!helper(node.left, lower, val)) {
            return false;
        }

    
        
        return true;
    }
}

```

#### 2.3.3.2 解法2：中序遍历，时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        Stack<TreeNode> stack = new Stack<>();
        TreeNode preNode = null;
        
        while (!stack.isEmpty() || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }

            root = stack.pop();
            if (preNode != null && root.val <= preNode.val){
                return false;
            }

            preNode = root;
            root = root.right;
        }

        return true;
    }
}


```





# 3.链表 

## 3.1 两两交换链表中的节点(2020-09-25第一遍，2020-09-27第二遍)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例:

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

### 3.1.2 基本思路

将三个看为一组，进行递归 a->b->c->d->e,abc就是一组，cde就是一组，终止条件就是head=null or head.next = null

### 3.1.3 代码实现

#### 3.1.3.1 解法1：递归实现

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }

        ListNode next = head.next;
        head.next = swapPairs(next.next);
        next.next = head;

        return next;
    }
}


```


#### 3.1.3.2 解法2：非递归实现

```
class Solution {
    public ListNode swapPairs(ListNode head) {
       //非递归解法
        ListNode pre = new ListNode(-1);
        pre.next = head;
        ListNode temp = pre;
        while(temp.next != null && temp.next.next != null){
            ListNode start = temp.next;
            ListNode end = temp.next.next;
            start.next = end.next;
            end.next = start;
            temp.next = end;
            temp = start;
        }

        return pre.next;
     
    }
}

```

## 3.2 环形链表 (完全会了)

### 3.2.1 题目描述

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

示例 1：

![image](http://note.youdao.com/yws/res/43428/731F167AFCD5450EAC8A697B75EDAB54)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

### 3.2.2 基本思路

### 3.2.3 代码实现

#### 3.2.3.1 快慢指针，解法1:时间复杂度O(n),空间复杂度O(1)

```
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }

        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (fast == slow) {
                return true;
            }
        }

        return false;
    }
}

```

#### 3.2.3.2 集合法，解法1:时间复杂度O(n),空间复杂度O(n)


```
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }

        Set<ListNode> set = new HashSet<>();
        while (head.next != null) {
            if (set.contains(head)) {
                return true;
            }

            set.add(head);
            head = head.next;
        }

        return false;
    }

}
```

# 4.栈和队列


## 4.1 最小栈(2020-09-27日第一遍)

### 4.1.1 题目描述

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。

pop() —— 删除栈顶的元素。

top() —— 获取栈顶元素。

getMin() —— 检索栈中的最小元素。

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/min-stack
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```

### 4.1.2 基本思路

### 4.1.3 代码内容

#### 4.1.3.1 解法1:时间复杂度O(1)，空间复杂度O(n)

```
import java.util.Stack;

//leetcode submit region begin(Prohibit modification and deletion)
class MinStack {

    Stack<Integer> minStack;
    Stack<Integer> stack;

    /**
     * initialize your data structure here.
     */
    public MinStack() {
        minStack = new Stack<>();
        stack = new Stack<>();
    }

    public void push(int x) {
        if (stack.isEmpty()) {
            stack.push(x);
            minStack.push(x);
        } else {
            stack.push(x);
            if (x <= minStack.peek()) {
                minStack.push(x);
            }
        }
    }

    public void pop() {
        int popVal = stack.pop();
        if (popVal == minStack.peek()) {
            minStack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```

#### 4.1.3.2 解法2，时间复杂度O(1),空间复杂度O(n)

```
//leetcode submit region begin(Prohibit modification and deletion)
class MinStack {

    Stack<Integer> stack;

    /**
     * initialize your data structure here.
     */
    public MinStack() {
        stack = new Stack<>();
    }

    public void push(int x) {
        if (stack.isEmpty()) {
            stack.push(x);
            stack.push(x);
        } else {
            int curMin = stack.peek();
            stack.push(x);
            if (curMin > x) {
                stack.push(x);
            } else {
                stack.push(curMin);
            }
        }
    }

    public void pop() {

    }

    public int top() {
    }

    public int getMin() {
    }
}

```

## 4.2 有效的括号(2020-09-27日第一遍)

### 4.2.1 题目描述

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 注意空字符串可被认为是有效字符串。

示例 1:

```
输入: "()"
输出: true
```

示例 2:


```
输入: "()[]{}"
输出: true
```

示例 3:


```
输入: "(]"
输出: false
```

示例 4:


```
输入: "([)]"
输出: false
```

### 4.2.2 基本思路

### 4.2.3 代码内容

#### 4.2.3.1 解法1：时间复杂度O(n),空间复杂度O(n)

```
import java.util.HashMap;
import java.util.Stack;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() == 0 || s.length() % 2 != 0) {
            return false;
        }

        Map<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');

        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ele = s.charAt(i);
            if (map.containsKey(ele)) {
                if (stack.isEmpty() || stack.peek() != map.get(ele)) {
                    return false;
                }

                stack.pop();
            } else {
                stack.push(ele);
            }
        }

        return stack.isEmpty();
    }
}

```



# 5.树

## 5.1 二叉树的中序遍历(10月4日第一遍)

### 5.1.1 题目描述

### 5.1.2 基本思路

### 5.1.3 代码实现

#### 5.1.3.1 解法1:递归实现,时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }

        inorderTraversal(root.left);
        res.add(root.val);
        inorderTraversal(root.right);

        return res;
    }
}

```

#### 5.1.3.2 解法2:非递归实现,时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }

        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }

            root = stack.pop();
            res.add(root.val);
            root = root.right;
        }

        return res;
    }
}
```

## 5.2 二叉树的先序遍历(10月4日第一遍)

### 5.2.1 题目描述

给定一个二叉树，返回它的 前序 遍历。


```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```

### 5.2.2 基本思路

### 5.2.3 代码实现

#### 5.2.3.1 解法1：递归实现，时间复杂度O(n)，空间复杂度O(1)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }

        res.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);

        return res;
    }
}
```

#### 5.2.3.2 解法2：非递归实现，时间复杂度O(n)，空间复杂度O(1)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }

        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                res.add(root.val);
                stack.push(root);
                root = root.left;
            }

            root = stack.pop();
            root = root.right;
        }


        return res;
    }
}


```

## 5.3 二叉树的后序遍历

### 5.3.1 题目描述

给定一个二叉树，返回它的 后序 遍历。

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```

### 5.3.2 基本思路

### 5.3.3 代码实现

#### 5.3.3.1 解法1:递归实现，时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }

        postorderTraversal(root.left);
        postorderTraversal(root.right);
        res.add(root.val);

        return res;
    }
}

```


#### 5.3.3.2 解法2:非递归实现，时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) {
            return res;
        }
        
        Stack<TreeNode> stack = new Stack<>();
        TreeNode preNode = null;
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if ((node.left == null && node.right == null) || 
                (preNode != null && (preNode == node.left || preNode == node.right))) {
                    res.add(node.val);
                    preNode = node;
                    stack.pop();
            } else {
                if (node.right != null) {
                    stack.push(node.right);
                }
                if (node.left != null) {
                    stack.push(node.left);
                }
            }
        }
        return res;
    }
}

```

## 5.4 二叉树的层序遍历

### 5.4.1 题目描述

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

```
示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

```

### 5.4.2 基本思路

### 5.4.3 代码实现

#### 5.4.3.1 解法1:时间复杂度O(n)，空间复杂度O(n)

```
class Solution {

    private List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return res;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> curList = new ArrayList(size);
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                curList.add(node.val);

                if (node.left != null) {
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }

            res.add(curList);
        }

        return res;
    }
}

```

## 5.5 N叉树的层序遍历

### 5.5.1 题目描述

### 5.5.2 基本思路

### 5.5.3 代码实现

#### 5.5.3.1 解法1:递归实现，时间复杂度O(n),空间复杂度O(n)

```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> postorder(Node root) {
        if (root == null) {
            return res;
        }

        for (Node cur:root.children) {
            postorder(cur);
        }

        res.add(root.val);

        return res;
    }
}
```

#### 5.5.3.2 解法2:非递归实现，时间复杂度O(n),空间复杂度O(n)


```
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> postorder(Node root) {
        if (root == null) {
            return res;
        }

        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            Node cur = stack.pop();
            res.add(cur.val);
            for (Node child: cur.children) {
                stack.push(child);
            }
        }

        Collections.reverse(res) ;
        return res;
    }
}

```



## 5.X

您需要在二叉树的每一行中找到最大的值。

示例：

输入: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]

class Solution{
    public List<Integer> largestValues(TreeNode root){
        List<Intger> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        List<Integer> values = new ArrayList<>();
        if (root != null)
            queue.add(root);
            
        while (!queue.isEmpty()) {
            int max = Integer.MIN_VALUE;
            int levelSize = queue.size();//层数
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();//出队
                max = Math.max(max,node.val);
                if (node.left != null)
                    queue.add(node.left);
                if (node.right != null)
                    queue.add(node.right);
            }
            values.add(max);
        }
        
        return values;
    
}

# 6.堆

## 6.1 最小的k个数(2020-10-09第一遍,2020-10-10第二遍)

### 6.1.1 题目描述

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

### 6.1.2 基本思路

### 6.1.3 代码内容

#### 6.1.3.1 解法1：排序法，时间复杂度O(nlogN)，空间复杂度O(k)

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0) {
            return new int[]{};
        }

        Arrays.sort(arr);
        int[] res = new int[k];
        
        for (int i = 0; i < k; i++) {
            res[i] = arr[i];
        }

        return res;
    }
}
```

### 6.1.3.2 解法2：堆，时间复杂度O(nlogK,int k),空间复杂度O(k)

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (arr == null || arr.length == 0 || k == 0) {
            return new int[]{};
        }

        int[] res = new int[k];
       PriorityQueue<Integer> queue = new PriorityQueue<Integer>(new Comparator<Integer>() {
            public int compare(Integer num1, Integer num2) {
                return num2 - num1;
            }
        });

        for (int i = 0; i < k; i++) {
            queue.offer(arr[i]);
        }

        for (int i = k; i < arr.length; i++) {
            if (queue.peek () > arr[i]) {
                queue.poll();
                queue.offer(arr[i]);
            }
        }

        for (int i = 0; i < k; i++) {
            res[i] = queue.poll();
        }

        return res;
    }
}

```

## 6.2 滑动窗口最大值(2020-10-09第一遍,2020-10-10第二遍)

### 6.2.1 题目描述

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 
```

### 6.2.2 基本思路

### 6.2.3 代码内容

#### 6.2.3.1 解法1:堆，时间复杂度O(nlogk),空间复杂度O(k)

```
  class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return new int[]{};
        }

        int[] res = new int[nums.length - k + 1];
        PriorityQueue<Integer> queue = new PriorityQueue<>(
            new Comparator<Integer>(){
                public int compare(Integer num1,Integer num2){
                    return num2 - num1;
                }
            });
        
        int index = 0;
        while (index < nums.length - k + 1) {
            for (int i = index; i < index + k; i++) {
                queue.offer(nums[i]);
            }

            res[index++] = queue.poll();
            while(!queue.isEmpty()){
                queue.poll();
            }
        }

        return res;
    }
}

```


#### 6.2.3.2 解法2:双端队列,时间复杂度O(N),空间复杂度O(k)

```
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return new int[]{};
        }

        int index = 0;
        int[] res = new int[nums.length - k + 1];
        LinkedList<Integer> queue = new LinkedList<>();

        for (int i = 0; i < k - 1; i++) {
            while (!queue.isEmpty() && nums[queue.peekLast()] <= nums[i]) {
                queue.pollLast();
            }

            queue.addLast(i);
        }

        for (int i = k - 1; i < nums.length; i++) {
            while (!queue.isEmpty() && nums[queue.peekLast()] <= nums[i]) {
                queue.pollLast();
            }

            queue.addLast(i);
            if (!queue.isEmpty() && i - queue.peekFirst() + 1 > k) {
                queue.removeFirst();
            }

            res[index++] = nums[queue.getFirst()];
        }

        return res;
    }
}
```

