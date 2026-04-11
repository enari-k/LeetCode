## 問題リンクはこちら
https://leetcode.com/problems/reverse-linked-list/description/

## step1(まずは解いてみる)
本当にお恥ずかしいのですが、自分は情報工学系の出身ではないというのもあり、LinkedListの問題を解いたことがなく、それも手伝ってLinkedListについて理解するところから始めました。
最初に思いついた愚直解がStackにすべてを格納したうえで、順に取り出してつなげていくというものです。先頭のデータを保持しておくために、dummyを使用しています。
この解法は、空間計算量が$O(N)$、時間計算量が$O(N)$となります。

```C#
public class Solution {
    public ListNode ReverseList(ListNode head) {
        Stack<int> Chest = new();
        while(head != null)
        {
            Chest.Push(head.val);
            head = head.next;
        }
        ListNode dummy = new ListNode(0);
        ListNode currentNode = dummy;
        while(Chest.Count != 0)
        {
            currentNode.next = new ListNode(Chest.Pop());
            currentNode = currentNode.next;
        }
        return dummy.next;
    }
}
```

上記の解法は直観的でぱっと思いつく内容だが、LinkedListならではの解法があるはずだと考えて、下記の内容を思いつく。
つまり前のノードと今のノードと先のノードを保持しつつ、今のノードをずらしながら、矢印の向きを変えていくのだ。
下記の内容は空間計算量が$O(1)$、時間計算量が$O(N)$となる。

```C#
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if(head == null)
        {
            return head;
        }
        ListNode PreNode = null;
        ListNode CurrentNode = head;
        ListNode NextNode;
        while(CurrentNode.next != null)
        {
            NextNode = CurrentNode.next;
            CurrentNode.next = PreNode;
            PreNode = CurrentNode;
            CurrentNode = NextNode;
        }
        CurrentNode.next = PreNode;
        return CurrentNode;
    }
}
```

## step2(清書)
参考にしたプルリク
https://github.com/attractal/leetcode/pull/7
https://github.com/miyataka/coding-practice/pull/7
https://github.com/rimokem/arai60/pull/7

プルリクをみて、そういえばpreが略されたものだったということを思い出す。
単語を下手に略すと可読性を下げるのでPreNode→PreviousNodeに変更。

```C#
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode PreviousNode = null;
        ListNode CurrentNode = head;
        ListNode NextNode = head;
        while(NextNode != null)
        {
            NextNode = CurrentNode.next;
            CurrentNode.next = PreviousNode;
            PreviousNode = CurrentNode;
            CurrentNode = NextNode;
        }
        return PreviousNode;
    }
}
```

自分では、思いつかなかったが再帰での解法が存在するみたいなので、そっちも実装。
一応、空間計算量の削減を意識して、末尾再帰で書いたがC#は末尾再帰の最適化に対応していないので、あまり意味はない。
時間計算量は$O(N)$空間計算量は$O(N)$ただし、もし同様のアルゴリズムを末尾再帰を保証している言語例えば、F#などで書いていれば、空間計算量は$O(1)$となった。

```C#
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if(head == null || head.next == null)
        {
            return head;
        }
        ListNode NextNode = head.next;
        head.next = null;
        return Reverse(head , NextNode);
    }
    private ListNode Reverse(ListNode PreviousNode, ListNode CurrentNode) {
        if(CurrentNode.next == null)
        {
            CurrentNode.next = PreviousNode;
            return CurrentNode;
        }
        ListNode NextNode = CurrentNode.next;
        CurrentNode.next = PreviousNode;
        return Reverse(CurrentNode , NextNode);
    }
}
```

## step3(復習)


