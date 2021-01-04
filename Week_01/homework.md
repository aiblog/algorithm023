用 add first 或 add last 这套新的 API 改写 Deque 的代码

Deque<String> deque = new LinkedList<String>();
deque.addFirst("a"); 
deque.addLast("b");   
deque.push("c");       
deque.addFirst("d");   
deque.addLast("e");    
deque.push("f");        

删除排序数组中的重复项

/*
    题目：在不创建新数组的条件下，在原数组中删除重复出现的数字。
    PS:数组是引用传递的，传递的是数组的头节点。对数组的修改会对调用者产生影响。
    !只修改前几个数就可以了
    方法一：快慢指针。题目中的数组是排序过了的，不需要单独排序。如果没排序过就Arrays.sort()
        left慢指针，right快指针。
        left左边是处理过的，right右边是未处理过的。
        由right遍历一遍数组。left记录下一个没有重复的数放置的位置。
    时间复杂度：O(n)
    空间复杂度：O(1)    
*/
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;
        // 快慢指针。left慢指针。right快指针。
        // 0到left是非重复元素。right到length-1是待检测元素
        int left = 0;
        for (int right = 1; right < nums.length; right++) {
            // 指针不相等时，将右指针的值放入左指针+1的位置
            if (nums[left] != nums[right]) {
                nums[++left] = nums[right];
            }    
        }           
        return left + 1;
    }
}


旋转数组



/*
    题目：将数组中的整体元素向右移动k个位置。
    方法一：暴力。
        i -> 0, k-1
            j -> 0, length -1        
                移动k次。没个数字都移动一遍。交换位置。
        时间复杂度O(n*k)
        空间复杂度O(1) 不需要额外空间
    方法二：建立新数组。
        先将原数组的数据拷贝到新数组中。
        再将新数组中的数据拷贝到原数组中。
        时间复杂度O(n*2)
        空间复杂度O(n) 
    方法三：将数字做反转。有点像是公式。！这个解法最精妙。
        先将整个数组反转。
        然后将0,k-1的数字反转
        再将k,len - 1的数字反转。
        就是所要的结果。
        时间复杂度O(n*2)
        空间复杂度O(1)
    方法四：直接将数字放到最后的结果位置，相应间隔的都换位置。然后下一个，同样相应间隔的都换位置。数量达到数组个数退出。
        时间复杂度：O(n)
        空间复杂度：O(1)
*/
/*
public class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;    // 对k取模，重复的圈数不需要计算
        int count = 0;          // 计数位，达到数组个数就完成了
        for (int start = 0; count < nums.length; start++) {
            int current = start;        // 当前位置
            int prev = nums[start];     // 出发值
            do {
                int next = (current + k) % nums.length; // 目标位置  
                // 交换                                                      
                int temp = nums[next];                  // 目标位置的值
                nums[next] = prev;                      // 目标位置的值改为出发值
                prev = temp;                            // 出发值改为目标位置的值
                current = next;                         // 当前索引改为目标位置的索引
                count++;                                // 计数+1
            } while (start != current);                 // 相对应的间隔位的数字都会移动到目标位置
        }
    }
}
*/
public class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}


合并两个有序链表

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
        题目：合并两个升序链表（已经升序处理过了），数字从小到大
        解法一：递归
            子问题与原问题有相同的结构
            时间复杂度: O(n+m)
            空间复杂度: O(n+m)
        解法二：暴力迭代：比较大小，然后改指向
            时间复杂度：O(n+m)
            空间复杂度：O(1)
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;                  // 链表1空，返回链表2
        else if (l2 == null) return l1;             // 链表2空，返回链表1
        else if (l1.val < l2.val) {                 // 链表1的值小
            l1.next = mergeTwoLists(l1.next, l2);   // 链表1指向 （链表1的下一个值与链表2合并）的结果
            return l1;                              // 返回链表1
        }
        else {                                      // 链表2的值小
            l2.next = mergeTwoLists(l1, l2.next);   // 链表2指向 （链表1与链表2的下一个值合并）的结果
            return l2;                              // 返回链表2                  
        }
    }
}

 

合并两个有序数组

（Facebook 在半年内面试常考）

/*
    题目：将数组2放入数组1中，并且排序。
        感觉这题靠API的使用。
        解法一：先合并，后排序
            System.arraycopy(nums2, 0, nums1, m, n);
            Arrays.sort(nums1);
            时间复杂度 : O( (n+m) * log(n+m) )
            空间复杂度 : O(1)
        解法二：双指针
            时间复杂度 : O(n + m)
            空间复杂度 : O(m)
            拷贝数字1为新数组。
                两个指针，比较数组1和数组2的大小，小的放入新数组中。
                比较完，剩余的放在新数组后面。
        解法三：三指针，从后往前放。
            p指针指向要放置的位置。p1指向数组1比较的值。p2指向数组2比较的值。
            时间复杂度 : O(n + m)
            空间复杂度 : O(1)
*/
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    // two get pointers for nums1 and nums2
    int p1 = m - 1;
    int p2 = n - 1;
    // set pointer for nums1
    int p = m + n - 1;
    // while there are still elements to compare
    while ((p1 >= 0) && (p2 >= 0))
      // compare two elements from nums1 and nums2 
      // and add the largest one in nums1 
      nums1[p--] = (nums1[p1] < nums2[p2]) ? nums2[p2--] : nums1[p1--];
    // add missing elements from nums2
    System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
  }
}


两数之和

/*
    找出数组中两个数字满足目标值的两个数字
        方法一：暴力迭代
            i -> 0, len - 2
                j -> i+1, len - 1
                    nums[i] + nums[j] == target
        时间复杂度O(n^2)
        空间复杂度O(n)
        方法二： 利用哈希表，两次迭代
            第一次，将所有数字放入哈希表中，
            第二次，遍历哈希表，查找目标值-当前值的结果是否在哈希中
        时间复杂度O(n * 2)
        空间复杂度O(n)    
        方法三：利用哈希表，两次迭代
            其实只需要一次，先判断目标值-当前值的结果是否在哈希中，
                不存在，在把值放进去。
        时间复杂度：O(n)
        空间复杂度：O(n)

*/
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}


移动零

/*
    将数组中的0移动到最后，保持原来的非零数字的顺序。
        要求不能开辟新数组。
        方法一：
            开辟新数组。遍历一次，一边统计0的个数，一将非0放入新数组中。第二次将0追加到新数组中。不满足题目要求。
            time:  O(n * 2)
            space: O(n)
        方法二：
            开辟新数组。双指针。一个指针指向头，非0数字放入，一个指针指向尾，0放入。不满足题目要求。
            time:  O(n)
            space: O(n)
        方法三：
            一个指针。记录非0元素放置的位置target。遍历一次，非0就放入target的位置。
            time:  O(n)
            space: O(1)
*/
public class Solution {
    public void moveZeroes(int[] nums) {
        int target = 0;
        for(int i = 0; i < nums.length; i++) 
            // 遍历到的数字非0，放到目标位置。
            if(nums[i] != 0) {
                int tmp = nums[target];
                nums[target] = nums[i];
                nums[i] = tmp;
                target++;
            }
    }
}


加一

/*
    一个数字组成的数组。转成数字，然后+1。再转成数组
        方法一：从后往前遍历。
            目标值不是9，直接+1，结束。
            目标值是9，当前位置值变为0，下个数+1。
            最后还是没结束，说明碰到了9999，需要开辟新数组，头为1。
            time:  O(n)
            space: O(n+1)
*/
class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length;
        // 从后往前遍历
        for (int i = len-1; i >= 0; i--) {
            // 数字不是9，直接自增，返回结果。
            if (digits[i] != 9) {
                digits[i]++;
                return digits;
            }
            // 是9，当前值变为0。继续遍历，把下个值+1。
            digits[i] = 0;
        }
        // 没结束，说明碰到了9999这种情况，开辟新数组，数组头写成1。
        int[] newNumber = new int [len+1];
        newNumber[0] = 1;
        return newNumber;
    }
}


设计循环双端队列

/*
    构造双端队列。用链表，两个指针。两个头尾哨兵
 */
class DoubleListNode {
    DoubleListNode pre;             // 前指针
    DoubleListNode next;            // 后指针
    int val;                        // 值
    public DoubleListNode(int val) {
        this.val = val;
    }
}

class MyCircularDeque {
    int size;               // 实际个数
    int k;                  // 开辟大小
    DoubleListNode head;    // 虚拟头节点哨兵
    DoubleListNode tail;    // 虚拟尾节点哨兵

    /**
     * 初始化，大小为k。
     */
    public MyCircularDeque(int k) {
        head = new DoubleListNode(-1);  // 头节点，默认值-1
        tail = new DoubleListNode(-1);  // 为节点，默认值-1
        head.pre = tail;                    // 头节点前指针指向尾
        tail.next = head;                   // 尾节点后指针指向头
        this.k = k;
        this.size = 0;
    }

    /**
     * 将一个元素添加到双端队列头部。 如果操作成功返回 true。
     */
    public boolean insertFront(int value) {
        if (isFull()) return false;                        // 容量不够，返回false
        DoubleListNode node = new DoubleListNode(value);    // 创造新节点
        // 新节点后指针指向头。新节点前指针指向头节点的前一个。头节点的前一个后指针指向当前节点。头节点的前指针指向新节点。
        node.next = head;
        node.pre = head.pre;
        head.pre.next = node;
        head.pre = node;
        size++;
        return true;
    }

    /**
     * 将一个元素添加到双端队列尾部。如果操作成功返回 true。
     */
    public boolean insertLast(int value) {
        if (isFull()) return false;
        DoubleListNode node = new DoubleListNode(value);
        node.next = tail.next;
        tail.next.pre = node;
        tail.next = node;
        node.pre = tail;
        size++;
        return true;
    }

    /**
     * 从双端队列头部删除一个元素。 如果操作成功返回 true。
     */
    public boolean deleteFront() {
        if (isEmpty()) return false;    // 元素空了，返回false
        head.pre.pre.next = head;       // 头节点的前前一个后指针指向头节点
        head.pre = head.pre.pre;        // 头节点的前指针指向头节点的前前一个
        size--;
        return true;
    }

    /**
     * 从双端队列尾部删除一个元素。如果操作成功返回 true。
     */
    public boolean deleteLast() {
        if (isEmpty()) return false;
        tail.next.next.pre = tail;
        tail.next = tail.next.next;
        size--;
        return true;
    }

    /**
     * 从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
     */
    public int getFront() {
        return head.pre.val;
    }

    /**
     * 获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
     */
    public int getRear() {
        return tail.next.val;
    }

    /**
     * 检查双端队列是否为空。
     */
    public boolean isEmpty() {
        return size == 0;
    }

    /**
     * 检查双端队列是否满了。
     */
    public boolean isFull() {
        return size == k;
    }
}


接雨水


/*
    一个数组表示柱子。问能接到多少水。
    方法一：暴力
    方法二：DP
    方法三：栈
        后面的柱子比前面的柱子低，是无法积水的。
        用栈存储柱子下标。
    方法四：双指针
*/
public class Solution {
    public int trap(int[] height) {
        // 存索引下标
        Stack<Integer> stack = new Stack<>();
        int current = 0, sum = 0;
        while (current < height.length) {
            // 如果栈不空并且当前柱子高度大于栈顶柱子的高度，就进入循环
            while (!stack.empty() && height[current] > height[stack.peek()]) {
                int top = stack.pop();                                                      // 取出栈顶元素的索引
                if (stack.empty()) break;                                                   // 栈空就结束
                int distance = current - stack.peek() - 1;                                  // 两个柱子之间的距离
                int min = Math.min(height[current], height[stack.peek()] - height[top]);    // 取当前柱子和前两个柱子高度差
                sum += distance * min;
            }
            stack.push(current++);                                                          // 当前柱子索引入栈
        }
        return sum;
    }
}


