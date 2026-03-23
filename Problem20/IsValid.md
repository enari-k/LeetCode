## 問題リンクはこちら
https://leetcode.com/problems/valid-parentheses/description/


## Step1
自分は競技プログラミングかたプログラミングに入ったので、とりあえず手を動かしてみるみたいなことが苦手な所がある気がした。方針は可能な限り関数の上部で管理してメンテナンスしやすくするというもの。
変数名はあまりよくなさそう。posとposesとあるので、こんがらがりそう。また、ifが３つあるのも共通化できそうだがぱっと思いつかず。


```C#
public class Solution{
    public bool IsValid(string s){
        List<char> open_pos = new List<char>{ '(' , '{' , '[' };
        Stack<char> poses = new();
        foreach(char pos in s)
        {
            if(open_pos.Contains(pos))
            {
                poses.Push(pos);
            }
            else
            {
                if(pos==')'&&poses.Pop()!='(') return false;
                if(pos=='}'&&poses.Pop()!='{') return false;
                if(pos==']'&&poses.Pop()!='[') return false;
            }
        }
        if(poses.Count == 0) return true;
        else return false;
    }
}
```

## 参考にしたのは下記リンクの内容になります。
https://github.com/jjysogfy/arai60-202603/pull/1/changes

## 思ったこと
アドバイスを反映して、辞書でかっこを管理してifを共通化。また、上記参考コードではif２つで表しているが、||で書いた方が、計算量が減ったはずなのでこちらで実装する。ただ、可読性は下がっていそう。
HashMapの仕様的に、ContainsValueではなく、ContainsKeyを採用。
ifの中にifを含んでしまい、ネストが深くなって可読性が下がっている。
.Count()はLINQでこれは、GCが呼び出されやすくなってしまうので、.Countを採用。
posは誤解を招きそうなので、cに変更。

## Step2(清書)
```C#
public class Solution {
    public bool IsValid(string s) {
        Dictionary<char , char> closeToOpen = new Dictionary<char , char>
        {
            { ')', '(' },
            { '}', '{' },
            { ']', '[' }
        };

        Stack<char> openBracketStack = new();
        foreach (char c in s)
        {
            if(closeToOpen.ContainsKey(c))
            {
                if(openBracketStack.Count == 0 || openBracketStack.Pop() != closeToOpen[c])
                {
                    return false;
                }
            }
            else
            {
                openBracketStack.Push(c);
            }
        }
        return openBracketStack.Count == 0;
    }
}
```
