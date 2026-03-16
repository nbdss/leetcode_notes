# 206. 反转链表

**难度**: Easy
**分类**: 链表
**链接**: https://leetcode.cn/problems/reverse-linked-list/

## 题意

给定单链表的头节点 `head`，反转链表并返回新的头节点。

## 我犯的错误

### Bug 1：`head` 为空时崩溃

```python
tmp_next = head.next  # head 为 None 时直接报错
```

把 `nxt` 放到循环里面声明，就不需要在外面提前取值。

### Bug 2：循环末尾对 None 取 `.next`

```python
tmp_next = tmp_next.next  # tmp_next 为 None 时崩溃
```

报错信息：`AttributeError: 'NoneType' object has no attribute 'next'`

同样，`nxt` 放到循环开头每次重新取值，循环条件保证了 `tmp` 不为 `None`。

### Bug 3：拼写错误 `.nxt` vs `.next`

```python
tmp.nxt = tmp_front   # ❌ 创建了新属性，实际的 .next 没被修改
tmp.next = tmp_front  # ✅
```

Python 允许动态添加属性，所以 `.nxt` 不会报错，但链表没有真正翻转。

### Bug 4：没有 return / return 了错误的变量

- 没有 `return` → 默认返回 `None`
- `return head` → head 还指着原来的节点 1（现在是尾节点），输出 `[1]`
- `return tmp` → 循环结束后 tmp 是 `None`
- 应该 `return prev`（`tmp_front`），它才是新的头节点

## 正确解法

```python
def reverseList(self, head):
    prev = None
    curr = head
    while curr:
        nxt = curr.next   # 先存下一个
        curr.next = prev  # 翻转指向
        prev = curr       # prev 前进
        curr = nxt        # curr 前进
    return prev
```

## 复杂度

- 时间复杂度: O(n)
- 空间复杂度: O(1)

## Python 知识点

- **赋值是引用绑定，不是复制**：`tmp = head` 后两者指向同一个节点
- **改属性会互相影响，重新赋值只改自己的指向**：
  - `tmp.next = xxx` → 通过 `head` 也能看到变化
  - `tmp = nxt` → 只有 `tmp` 换了指向，`head` 不动
- **链表用头节点代表**：Python 没有专门的链表类，返回头节点就是返回整条链表
