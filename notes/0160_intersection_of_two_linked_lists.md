# 160. 相交链表

**难度**: Easy
**分类**: 链表、双指针
**链接**: https://leetcode.cn/problems/intersection-of-two-linked-lists/

## 题意

给定两个单链表的头节点 `headA` 和 `headB`，找出并返回两个链表相交的起始节点。如果不相交，返回 `None`。

## 我犯的错误

### 语法错误（Python 基础）

| 错误写法 | 正确写法 | 说明 |
|----------|----------|------|
| `null` | `None` | Python 没有 null |
| `&&` | `and` | Python 逻辑运算符 |
| `a[i] = x`（空列表）| `a.append(x)` | 空列表不能用下标赋值 |

### 逻辑错误

- **相交的定义是同一个对象，不是值相等**，不能用 `.val` 比较
- 数组存的是值（int）不是节点，没法返回 `ListNode`
- `return a[c]` 在第一次迭代时 `c = len(a)`，导致越界

## 正确解法：双指针

```python
def getIntersectionNode(headA, headB):
    a, b = headA, headB
    while a != b:
        a = a.next if a else headB
        b = b.next if b else headA
    return a
```

## 为什么正确

设链表 A 长度为 m，链表 B 长度为 n，公共部分长度为 k。

**有交叉时：**

```
A：  a1 → a2 → c1 → c2 → c3        长度 m = 2 + 3
B：  b1 → b2 → b3 → c1 → c2 → c3   长度 n = 3 + 3
```

- 指针 a 走完 A（m 步）跳到 B 头，再走到 c1，共 `m + (n - k)` 步
- 指针 b 走完 B（n 步）跳到 A 头，再走到 c1，共 `n + (m - k)` 步
- 两者步数相等：`m + n - k == n + m - k`，必然同时到达 c1

**无交叉时：**

- a 走了 `m + n` 步到达 `None`
- b 走了 `n + m` 步到达 `None`
- `None == None` 成立，退出循环返回 `None`

## 复杂度

- 时间复杂度: O(m + n)
- 空间复杂度: O(1)

## 总结

- 判断链表节点相同用 `==`（比较引用），不要比较 `.val`
- 双指针"互换起点"是链表相交问题的经典技巧，核心在于让两个指针走过相同的总路程
