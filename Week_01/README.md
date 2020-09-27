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

