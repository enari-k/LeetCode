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

## Step2.1(清書)

### 参考にしたのは下記リンクの内容になります。
https://github.com/jjysogfy/arai60-202603/pull/1/changes

### 思ったこと
アドバイスを反映して、辞書でかっこを管理してifを共通化。また、上記参考コードではif２つで表しているが、||で書いた方が、計算量が減ったはずなのでこちらで実装する。ただ、可読性は下がっていそう。
HashMapの仕様的に、ContainsValueではなく、ContainsKeyを採用。
ifの中にifを含んでしまい、ネストが深くなって可読性が下がっている。
.Count()はLINQでこれは、GCが呼び出されやすくなってしまうので、.Countを採用。
posは誤解を招きそうなので、cに変更。

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

## Step2.2(清書)
### 思ったこと
参考コードをもう一度見直した際、Dictionaryをconstにするのもありという話があったので、採用。とはいえ、C#では直接Dictionaryをconstにできないので、他の手段で代替。

本当はC# 12/.Net8の機能のFrozenDictionaryを使いたかったが、LeetCodeが対応していないということで、readonlyのメンバ変数として宣言。

欠点としてこの方法だと上書きを防げるが追加は防げないので、他の関数からの副作用が怖い。これを防ぐ手段として、ReadOnlyDictionaryを関数内で宣言するという手もあるが、今回はclassの中に関数があまりなく副作用が起こりにくいパターンということで、不採用。

副次的効果として、これまではIsValid()が呼び出されるたびにnewされていたのが、コンパイル時の一回しかインスタンスが作られないので早くなった。

また、アドバイスにあったように早期continueでifのネストを無くし、可読性を上げた。
本当は、||ではなくif二つで片方にcontinueをつければ同様の効果を得つつ||を排除して条件式を短くできるが、自分は異常系の処理は一つにまとまっていた方が読みやすいと思ったので、一つにまとめる。

ここは好みの問題？

```C#
public class Solution {
    private static readonly Dictionary<char , char> closeToOpen = new Dictionary<char , char>
    {
        { ')', '(' },
        { '}', '{' },
        { ']', '[' }
    };
    public bool IsValid(string s) {
        Stack<char> openBrackets = new();
        foreach (char c in s)
        {
            if(!closeToOpen.ContainsKey(c))
            {
                openBrackets.Push(c);
                continue;
            }
            if(openBrackets.Count == 0||openBrackets.Pop() != closeToOpen[c])
            {
                return false;
            }
        }
        return openBrackets.Count == 0;
    }
}
```