## 問題リンクはこちら
https://leetcode.com/problems/remove-duplicates-from-sorted-list/


## Step1(まずは解いてみる)
ポインターを動かして、前のやつと重複していたら無視して違う奴だったらポインターを付け替えるアルゴリズム。

previousNode.next = null;を忘れて、例えば、1→2→2などの場合、ポインターを付け替えるトリガーが動かないということを見落として、WAを出してしまう。

```cs
public class Solution {
    public ListNode DeleteDuplicates(ListNode head) {
        if (head == null)
        {
            return null;
        }
        ListNode currentNode = head;
        ListNode previousNode = head;
        while (currentNode != null)
        {
            if(previousNode.val != currentNode.val)
            {
                previousNode.next = currentNode;
                previousNode = currentNode;
            }
            currentNode = currentNode.next;
        }
        previousNode.next = null;
        return head;
    }
}
```

## Step2(清書)

参考にしたプルリク
https://github.com/nicah4o/arai60/pull/3/changes
https://github.com/hajimeito1108/arai60/pull/6/changes

他の方のコードを見たが、whileの中でelseだったり、whileの中にwhileを用いているものだったりだったので、自分のコードの方が簡潔にかけていると考えて、特に変更を加えなかった。

```cs
public class Solution {
    public ListNode DeleteDuplicates(ListNode head) {
        if (head == null)
        {
            return null;
        }
        ListNode currentNode = head;
        ListNode previousNode = head;
        while (currentNode != null)
        {
            if(previousNode.val != currentNode.val)
            {
                previousNode.next = currentNode;
                previousNode = currentNode;
            }
            currentNode = currentNode.next;
        }
        previousNode.next = null;
        return head;
    }
}
```

## Step3(復習)

