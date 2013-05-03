# ひとりでつかう - 二度目のコミット

さて、前回で、作業ディレクトリで変更した内容を stage し、コミットするところまで見てみました。では、今からで二度目のコミットを行ってみましょう。

なにはともあれ、まずは状態の確認から行いましょう。

    $ cd path/to/my_first_workspace
    $ git status
    # On branch master
    nothing to commit, working directory clean

はい、nothing to commit, woking directory clean だそうです。「コミットするものはなにもないし、作業ディレクトリもキレイなままだよ」と教えてくれてますね。リポジトリに登録されてる内容と、作業ディレクトリの間に変更がないよ、ということです。

## 作業ディレクトリで作業

では、作業ディレクトリで作業をしてみましょう。ディレクターから「猫の鳴き声さー、"にゃん"じゃなくてさ、"ミュウ"にしようよー」と言われたので、その変更を行いましょう。お好きなエディタで、nyan.txtを編集しましょう

    nyan

だったものを

    mew

と書き換えてください。書き換えましたか？

ではここで、おもむろに git status です

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   nyan.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")
    
はい、いろいろ出てきましたね。上から読んで行きましょう。

Changes not staged for commit のあとに括弧でごにょごにょ言って、その下に modified: nyan.txt です。

日本語にすれば、「コミット用にstageされてない変更は以下の通りです。nyan.txt が変更されています」くらいの感じでしょうか。もう意味わかりますね。作業ディレクトリの内容を変更したけど、staging area に上がってないわけですね。括弧の中身も見てみましょう。ふたつあります。 

「use "git add <file>..." to update what will be committed」。「"git add <file> ..." すると コミットされるようになるよ」とのこと。untracked なファイルを track するときと一緒ですね。こんな感じで、 git add で、編集したり新しくできたりしたファイルを staging area に登録することができます。

もうひとつのほう「use "git checkout -- <file>..." to discard changes in working directory」だそうです。「"git checkout -- <file> ..." すると、作業ディレクトリ内での変更をなかったことにできるよ！」だそうです。

へえー。

じゃあやってみましょう。

    $ git checkout -- nyan.txt

としたあとに、nyan.txt の中身を見てみてください。 

    nyan

に戻っていると思います。

おおー。当然ですが、git status をすれば「working directoryはcleanだよ」って言われます。

    $ git status
    # On branch master
    nothing to commit, working directory clean

おっと！寄り道をしてしまいました。せっかく編集したファイルをもとにもどしてしまったので、もう一度 nyan を mew に書き換えて、今度はコミットしてみましょう。

まずは nyan.txt の中身を書き換えてください。

書き換えましたか？ では git status で状態を確認。

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   nyan.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

git add で stage

    $ git add nyan.txt

git status で確認

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   nyan.txt
    #

はい、新しい表示が出てきました

もう説明しなくてもいいですか？ しますか？ しましょうか。

「Changes to be committed、括弧でごにょごにょ、 modified: nyan.txt」は「コミットされる変更は以下の通りだよ。・変更されたファイル：nyan.txt」でいいですね。括弧の中は「use "git reset HEAD <file>..." to unstage」だそうです。「"git reset HEAD <file> ..." ってやるとunstageできるよ」だそうです。

へー。

やってみましょう。

    $ git reset HEAD nyan.txt
    Unstaged changes after reset:
    M	nyan.txt

なんか出ましたね。「Unstaged changes after reset: M nyan.txt」だそうです。「resetしたあとにunstageされた変更は以下の通りだよ。M nyan.txt」ってことですね。M nyan.txt はなんとなく察しがついてるかと思いますが、「Modified」の M です。nyan.txtが変更されてるけどその変更が git reset によって unstage されたよって言ってくれてますね。

じゃあ git status で確認だ！

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   nyan.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")
    
はい、git add で nyan.txt を stage する前の状態に戻りました。

おっと、また寄り道してしまい、せっかく stage した変更を unstage してしまいました。ここからはノンストップで commit までやってしまいましょう。

    $ git add nyan.txt

で nyan.txt に対しておこなった変更を stage して、

    $ git commit

でエディタを立ち上がるので、「猫の鳴き声を nyan から mew に変更」みたいな感じのコミットメッセージを書いて変更、保存です。

    [master 66346b5] 猫の鳴き声を nyan から mew に変更
     1 file changed, 1 insertion(+), 1 deletion(-)

と表示されました。1 file changed, 1 insertion(+), 1 deletion(-) だってさ。つまり、「一つのファイルが変更されててー、一行挿入されててー、一行消えてる！」とのことです、一行編集するってのは言い換えれば一行消して一行挿入するってことなので、なるほどね！って感じですね。

## コミットオブジェクトふたたび

さて、これでリポジトリに存在するコミットオブジェクトはふたつになりました。ところで、この二つのコミットオブジェクトはどういう関係なのでしょうか？ じつは、特殊なコミットオブジェクトを除き、コミットオブジェクトは必ずひとつの「親となるコミットオブジェクト」を持っています。最初に行ったコミットはその特殊なコミットオブジェクトのうちのひとつで、これを「ルートコミット(オブジェクト)」と呼びます、最初にコミットを行うときには「親」となれるような他のコミットオブジェクトがないので、仕方なく「親なし」のコミットオブジェクトができあがるのですね。そして、今行った二度目のコミットにとっては、この最初のコミットが親となります。基本的に、「前やったコミット」が「今やったコミット」の親となります。

では、この親子関係は実際には何を表しているのでしょうか？ それを見てみましょう。

まず、「コミットする」という言葉が何を言っているのか、再度考えてみましょう、コミットするというのは、「変更内容をリポジトリに反映する」ということでしたね。では、ここで、「変更」という言葉に含まれる意味をちょっと考えてみましょう、「変更」と言うからには、必ず「以前の状態」があるはずです。そして、「その以前の状態に対して行われる変更」があって、その変更の結果として「現在の状況」が生まれるわけですね。コミットオブジェクトには、このうち、「以前の状態からどのような変更を行ったのか」の部分が入っています。「以前の状態」は入っていないし、「現在の状態」も入っていません。入っているのは、「以前の状態からどのように変更を行ったか」ということだけです。

さて、では、そのような断片的な情報から、どうやってリポジトリは「現在の状態」を知ることができるのでしょうか。ここで先ほどの「親子関係」の話が生きています。

まずは、最初のコミット、「ルートコミット」が生まれたときのことを考えてみましょう、ルートコミットには親がいないのでした。ルートコミットにとっての「以前の状態」というのは「なにもない状態」ですから、これはまあいいでしょう。そして、ルートコミットには「以前の状態」からどのような変更を行ったかが入っています。

このふたつの情報があれば、Git さんはいつでも「ルートコミットが生まれた瞬間」の状態を復元することができますね。「なにもな状態」のところに、ルートコミットの中にある「どのような変更を行ったのか」という情報を使って、同じ変更を行えばいいのです。

では、ここに二回目のコミットが生まれたときのことを考えてみましょう。二回目のコミットの親コミットはルートコミットです。これは、二回目のコミットのコミットオブジェクトが参照している「以前の状態」が、「ルートコミットが生まれた瞬間の状態」である、ということを意味しています。そして、二回目のコミットのコミットオブジェクトの中には、「以前の状態」からどのような変更を行ったのかが書かれています。

このふたつの情報をさらに使えば、Git さんはいつでも「二回目のコミットが生まれた瞬間」の状態を復元することができます。まずは先ほどみた通り「ルートコミットが生まれた瞬間」を復元して、その状態から、今度は二回目のコミットオブジェクトの中にある「そこからどのような変更を行ったのか」という情報を使って、同じ変更を行えば良いのです。

同様に、三回目、四回目とコミットされたとしても、Git さんはそのコミットオブジェクトの中にある「以前の状態からどういう変更を行ったのか」という情報と、「以前の状態」を表すコミットはどれかという情報を使うことによって、任意のコミットの瞬間の状態を復元できるわけです。

賢い！！！！

## 履歴の確認

さて、これで二度目のコミットが行われました。ログを確認しておきましょう。

    $ git log
    commit 66346b53f10bbe68efb14d72a56eea836dd7e0f2
    Author: Shinpei Maruyama <shinpeim@gmail.com>
    Date:   Sat May 4 03:20:59 2013 +0900

        猫の鳴き声を nyan から mew に変更

    commit 33028c115cc19cf6122fc2004fc6393a22712d23
    Author: Shinpei Maruyama <shinpeim@gmail.com>
    Date:   Fri May 3 22:32:52 2013 +0900

        猫の鳴き声を管理するファイルを作成
        
twitterのタイムラインと一緒で、あたらしいものほど上に来ています。ふたつのコミットがあるのが確認できますね。--graph というオプションをつけて実行すると、コミットの親子関係を視覚化することもできます。

    $ git log --graph
    * commit 66346b53f10bbe68efb14d72a56eea836dd7e0f2
    | Author: Shinpei Maruyama <shinpeim@gmail.com>
    | Date:   Sat May 4 03:20:59 2013 +0900
    | 
    |     猫の鳴き声を nyan から mew に変更
    |  
    * commit 33028c115cc19cf6122fc2004fc6393a22712d23
      Author: Shinpei Maruyama <shinpeim@gmail.com>
      Date:   Fri May 3 22:32:52 2013 +0900
  
      猫の鳴き声を管理するファイルを作成
 
\* と * を | でつなぐことによって、このふたつのコミットが親子関係にあることが示されています。git log の表示フォーマットはいろいろいじれるので、気になるひとは調べてみて、自分の好きなフォーマットで見てみるといいと思います。

## まとめ

今回は二度目のコミットを行ってみました。それにより、

* コミットには親子関係がある
* コミットオブジェクトには「親の状態からどのような変更を行ったのか」という情報が入っている
* これらのふたつの情報をつなぎ合わせることによって、Git さんはいつでも「そのコミットが生まれたときの状態」を復元することが可能となっている

ということが学べました。

次回は「ブランチ」について見てみましょう。