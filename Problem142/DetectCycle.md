## Stap1 まずは解いてみる
まずは、愚直解で解いてみる。つまり、順に探索してだんだんとHashSetに格納していくと、初めに重複したListNodeがサイクルの開始地点であるはずである。時間計算量はO(N)、空間計算量もO(N)となる。

```C#
public class Solution {
    public ListNode DetectCycle(ListNode head) {
        HashSet<ListNode> visited_Nodes = new();
        while(head != null)
        {
            if(visited_Nodes.Contains(head))
            {
                return head;
            }
            visited_Nodes.Add(head);
            head = head.next;
        }
        return null;
    }
}
```

H:ループの開始地点
L:一ループの長さ
D:ループの開始地点から衝突地点まで
のように定義する。この時、Problem141-Linked List Cyleで使ったアルゴリズムを思い出すと、fast == slowの時、
fast = H + D + n * L
slow = H + D
となるはずである。ここで、fast = slow * 2が成立することから
H + D + n * L = 2 * (H + D)
より整理すると、
H = n * L - D
H = (n - 1) * L + (L - D)
となる。上記の式から先ほどの衝突地点から、H歩進んだ時に、ちょうどループしてループの開始地点に戻ることが分かる。よって、開始地点からH歩進ませるのとさっきの衝突地点からH歩進ませたものはちょうどループの開始地点で衝突するはずである。これによって、ループの開始地点は特定できる。
時間計算量はO(N)、空間計算量はO(1)となる。
二回目のループで、一回目のループと同様にfast==slowの判定よりも前に、ListNodeを進める処理を入れてしまいエラーを出してしまう。ループで最初に戻る場合、インデックス0地点ではなく、合流後一歩進んでから判定されてしまうというのを見落としていた。
if(head == null) return null;を挟み忘れてエラーを出してしまう。そのようなケースはwhileのfast.next != nullではじかれると思っていたが、よく考えると、fast.nextでnullを参照してしまっている。

```C#
public class Solution {
    public ListNode DetectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        bool isCycle = false;
        if(head == null) return null;
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
        if(!isCycle) return null;
        slow = head;
        while(true)
        {
            if(fast == slow)
            {
                return fast;
            }
            slow = slow.next;
            fast = fast.next;
        }
    }
}
```

## Step2(清書)

参考にしたプルリク
https://github.com/hiro111208/leetcode/pull/2/changes
https://github.com/Nbotter/leetcode-easy-swe-practice/pull/12/changes

C#の変数名はvisited_Nodesなど_で区切るのではなくvisitedNodesというように単語の区切りを大文字にして区切るのがcamelCaseだと思い出し、そのように修正。

```C#
public class Solution {
    public ListNode DetectCycle(ListNode head) {
        HashSet<ListNode> visitedNodes = new();
        while(head != null)
        {
            if(visitedNodes.Contains(head))
            {
                return head;
            }
            visitedNodes.Add(head);
            head = head.next;
        }
        return null;
    }
}
```

while(true)を入れると、本当にどのケースでも正しく終了するのかという認知負荷を読み手に課すことに気付いたので、whileの終了条件をfast == slowに変更。

```C#
public class Solution {
    public ListNode DetectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        bool isCycle = false;
        while(fast != null && fast.next != null)
        {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow)
            {
                isCycle = true;
                break;
            }
        }
        if(!isCycle) return null;
        slow = head;
        while(fast != slow)
        {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
}
```

## Step3(復習)

