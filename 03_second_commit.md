# ひとりでつかう - にどめのコミット

さて、前回、作業ディレクトリで変更した内容を stage し、コミットするところまで見てみました。では、今からで二度目のコミットを行ってみましょう。

なにはともあれ、まずは状態の確認から行いましょう。

    $ cd path/to/my_first_workspace
    $ git status
    # On branch master
    nothing to commit, working directory clean

はい、nothing to commit, woking directory clean だそうです。「コミットするものはなにもないし、作業ディレクトリもキレイなままだよ」と教えてくれてますね。リポジトリに登録されてる内容と、作業ディレクトリの間に変更がないよ、ということです。

## 作業ディレクトリで作業

では、作業ディレクトリで作業をしてみましょう。ディレクターから「猫の鳴き声さー、"にゃん"じゃなくてさ、"ミュウ"にしようよー」と言われたので、その変更を行いましょう。お好きなエディタで、nyan.txt を編集しましょう

    nyan

だったものを

    mew

と書き換えてください。書き換えましたか？

ではここで、おもむろに `git status` です

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

「Changes not staged for commit:」のあとに括弧でごにょごにょ言って、その下に 「modified: nyan.txt」です。

日本語にすれば、「コミット用に stage されてない変更は以下の通りです。変更されたファイル：nyan.txt」くらいの感じでしょうか。もう意味わかりますね。作業ディレクトリの内容を変更したけど、そのファイルのその内容が staging area に上がってないわけですね。括弧の中身も見てみましょう。ふたつあります。 

「use "git add \<file\>..." to update what will be committed」。「"git add \<file\> ..." すると コミットされるようになるよ」とのこと。untracked なファイルを track するときと一緒ですね。こんな感じで、 編集したり新しくできたりしたファイルを `git add` で staging area に登録することができます。

もうひとつのほうの括弧の中も見てみましょう。「use "git checkout -- \<file\>..." to discard changes in working directory」だそうです。「"git checkout -- \<file\> ..." すると、作業ディレクトリ内での変更をなかったことにできるよ！」だそうです。

へえー。

じゃあやってみましょう。

    $ git checkout -- nyan.txt

としたあとに、nyan.txt の中身を見てみてください。 

    nyan

に戻っていると思います。おおー。

当然ですが、この状態で git status をすれば「working directory は clean だよ」って言われます。

    $ git status
    # On branch master
    nothing to commit, working directory clean

おっと！寄り道をしてしまいました。せっかく編集したファイルをもとにもどしてしまったので、もう一度 nyan を mew に書き換えて、コミットしてみましょう。

まずは nyan.txt の中身を書き換えてください。

書き換えましたか？ では `git status` で状態を確認。

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   nyan.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

`git add` で stage

    $ git add nyan.txt

`git status` で確認

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   nyan.txt
    #

はい、新しい表示が出てきました

「Changes to be committed、括弧でごにょごにょ、 modified: nyan.txt」だそうです。「コミットされる変更は以下の通りだよ。変更されたファイル：nyan.txt」でいいですね。括弧の中は「use "git reset HEAD \<file\>..." to unstage」とあります。「"git reset HEAD \<file\> ..." ってやるとunstageできるよ」ってことですね。

へー。

試しにやってみましょう。

    $ git reset HEAD nyan.txt
    Unstaged changes after reset:
    M	nyan.txt

なんか出ましたね。「Unstaged changes after reset: M nyan.txt」だそうです。「unstageされた変更は以下の通りだよ。M nyan.txt」ってことですね。M nyan.txt はなんとなく察しがついてるかと思いますが、「Modified」の M です。nyan.txtが変更されてるけど、その変更が `git reset` によって unstage されたよって言ってくれてますね。

じゃあ `git status` で確認だ！

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   nyan.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")
    
はい、`git add` で nyan.txt を stage する前の状態に戻っている(= unstageされた)ことが確認できました。

おっと、また寄り道してしまい、せっかく stage した変更を unstage してしまいました。ここからはノンストップで commit までやってしまいましょう。

    $ git add nyan.txt

で nyan.txt に対しておこなった変更を stage して、

    $ git commit

でエディタが立ち上がるので、「猫の鳴き声を nyan から mew に変更」みたいな感じのコミットメッセージを書いて変更、保存です。

    [master 66346b5] 猫の鳴き声を nyan から mew に変更
     1 file changed, 1 insertion(+), 1 deletion(-)

と表示されました。「1 file changed, 1 insertion(+), 1 deletion(-)」だってさ。つまり、「一つのファイルが変更されててー、一行挿入されててー、一行消えてる！」とのことです、一行編集するってのは言い換えれば一行消して一行挿入するってことなので、なるほどね！って感じですね。

## コミットオブジェクトふたたび

さて、2 回コミットを行ったので、これでリポジトリに存在するコミットオブジェクトはふたつになりました。ところで、この二つのコミットオブジェクトはどういう関係なのでしょうか？ じつは、特殊なコミットオブジェクトを除き、コミットオブジェクトは必ずひとつの「親となるコミットオブジェクト」を持っています。最初に行ったコミットはその特殊なコミットオブジェクトのうちのひとつで、これを「ルートコミット(オブジェクト)」と呼びます、最初にコミットを行うときには「親」となれるような他のコミットオブジェクトがないので、仕方なく「親なし」のコミットオブジェクトができあがるのですね。そして、今行った二度目のコミットにとっては、この最初のコミットが親となります。基本的に、「前やったコミット」が「今やったコミット」の親となります。

では、この親子関係は実際には何を表しているのでしょうか？ それを見てみましょう。

まず、「コミットする」という言葉が何を言っているのか、再度考えてみましょう、コミットするというのは、「変更内容をリポジトリに反映する」ということでしたね。では、ここで、「変更」という言葉に含まれる意味をちょっと考えてみましょう、「変更」と言うからには、必ず「以前の状態」があるはずです。そして、「以前の状態に対して行われる変更」があって、その変更の結果として「現在の状態」が生まれるわけですね。

そして、コミットオブジェクトには、「変更後の状態」が入っています。「以前の状態」は入っていないし、「どのような変更を行ったのか」も入っていません。入っているのは、「変更後の状態」だけ、ということだけです。

さて、では、そのような断片的な情報から、どうやってリポジトリは「そのコミットでどのような変更が行われたのか」を知ることができるのでしょうか。ここで先ほどの「親子関係」の話が生きています。

まずは、最初のコミット、「ルートコミット」が生まれたときのことを考えてみましょう、ルートコミットには親がいないのでした。ルートコミットにとっての「以前の状態」というのは「なにもない状態」ですから、これはまあいいでしょう。そして、ルートコミットが生まれたことにより、ルートコミットを参照することで「現在の状態」を確認できるようになりました。

さて、これで、ルートコミットはふたつの情報を手に入れました。

* ルートコミットの中に記されている状態
* 以前はなにもない状態であったということ

このふたつの情報があれば、Git さんは「ルートコミットに記されている状態」と「なにもなかったときの状態」のを比べることで、「ルートコミットでどのような変更が行われたのか」を知ることができます。

では、次は二度目のコミットが生まれたときのことを考えてみましょう。二度目のコミットの親コミットはルートコミットです。「親コミットがルートコミットである」というのはどういう意味なのでしょうか？ 実はこれは、二度目のコミットのコミットオブジェクトの「以前の状態」が、「ルートコミットが保持している内容」である、ということを意味しているのです。

さて、これで二度目のコミットはふたつの情報を手に入れました。

* 二度目のコミットの中に記されている状態
* 二度目のコミットが行われる以前の状態は、ルートコミットが知っているということ

このふたつの情報をさらに使えば、Git さんはいつでも「ルートコミットから 二度目のコミットの間に起こった変更」を知ることができます。簡単ですね、ルートコミットの持っている内容と 二度目のコミットの持っている内容を比べれば、その差分がまさに「変更された内容」です。

同様に、三度目、四度目とコミットされたとしても、Git さんは「そのコミットオブジェクトに保存されている状態」と、「親コミットオブジェクトが保持していた内容」を比べることで、「どんな変更が行われたか」を知ることができるのです。

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
 
\* と \* を | でつなぐことによって、このふたつのコミットが親子関係にあることが示されています。`git log` の表示フォーマットはいろいろいじれるので、気になるひとは調べてみて、自分の好きなフォーマットで見てみるといいと思います。

## まとめ

今回は二度目のコミットを行ってみました。それにより、

* コミットには親子関係がある
* コミットオブジェクトには「親コミットはどれか」という情報と、「この瞬間のファイルの状態」という情報が入っている
* これらのふたつの情報を比べることによって、Git さんはいつでも「その 2 つのコミットの間にどのような変更が行われたか」を知ることができる

ということが学べました。

次回はもっといろんなコミットを行ってみましょう。

[next - ひとりでつかう - どんどんコミット](04_more_commits.md)
