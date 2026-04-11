## 問題のリンクはこちらです
https://leetcode.com/problems/linked-list-cycle/

## Step1(とりあえず解いてみる)
全てをHashSetに格納していき、もう含まれていればfalseそうでなく、Linked Listの末尾まで到達し末尾の一つ後ろがnullなら
falseを返すような解法。HashMapによるO(1)の高速な検索が使えるHashSetを利用。時間計算量はO(N),空間計算量はO(N)。

```C#
public class Solution {
    public bool HasCycle(ListNode head) {
        HashSet<ListNode> DuplicateFlag = new();
        while(head != null)
        {
            if(DuplicateFlag.Contains(head))
            {
                return true;
            }
            DuplicateFlag.Add(head);
            head = head.next;
        }
        return false;
    }
}
```

自分では、思いつかなかったがウサギとカメのアルゴリズムを、参考にしたプルリクから見つけたので、それも実装する。時間計算量はO(N),空間計算量はO(1)。

```C#
public class Solution {
    public bool HasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        bool isCycle = false;
        if(head == null) return false;
        while(fast.next != null && fast.next.next != null)
        {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow)
            {
                isCycle = true;
                break;
            }
        }
        return isCycle;
    }
}
```

## Step2(清書)
参考にしたプルリク

https://github.com/froggyy351/LeetCode_arai60/pull/1/changes
https://github.com/hiro111208/leetcode/pull/1/changes

camelCaseから、最初を小文字にしたほうが良さそうなので、DuplicateFlag→duplicateFlagに変更。

```C#
public class Solution {
    public bool HasCycle(ListNode head) {
        HashSet<ListNode> duplicateFlag = new();
        while(head != null)
        {
            if(duplicateFlag.Contains(head))
            {
                return true;
            }
            duplicateFlag.Add(head);
            head = head.next;
        }
        return false;
    }
}
```

とりあえず解くでは、フラグ変数を使って結果を管理していたがそもそもその場で結果をreturnすれば簡潔に書けるということに気付いたので、そのように実装。
普段C#ではカーネルモードに都度切り替えると遅いという理由からAutoFlashをオフにして、最後にまとめてカーネルモードに切り替えて標準出力を返すというような実装をすることが多かったので、このような実装をしてしまったのだと思われる。

```C#
public class Solution {
    public bool HasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        if(head == null) return false;
        while(fast.next != null && fast.next.next != null)
        {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow)
            {
                return true;
            }
        }
        return false;
    }
}
```

## Step3(復習)