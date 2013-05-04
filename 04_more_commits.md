# ひとりでつかう - どんどんコミット

さて、前回までで、staging areaについてと、コミットオブジェクトについてなんとなくわかってもらえたのではないかと思います。再度まとめておきましょう。

* 作業ディレクトリでなんらかの変更を行う
* 行った変更のうち、コミット時にリポジトリに反映したいものを「staging area」に登録する
* commit を行いコミットメッセージを記入する
* コミットメッセージとともに、staging areaに登録されていた変更がコミットオブジェクトに詰め込まれ、それがリポジトリに保存される
* これにより、Gitさんは「あのときのコミットの状態を復元！」みたいなことができるようになる。

ところで、今まで見てきた「作業ディレクトリ内の変更」は、「ファイルの追加」と「ファイルの内容の編集」のふたつでしたね？しかし、作業を行っていく上でのディレクトリ内の変化は、もちろんそれだけではないでしょう。「ファイルの削除」と「ファイルのリネーム」「ファイルの移動」というような変化が起こることもあるでしょう。というわけで、今回は「ファイルの削除とリネーム、移動」について見て行きましょう。

## ファイルの削除 - その準備

今までと同じ作業ディレクトリにて作業を行います。とはいえ、いまは nyan.txt しかリポジトリに存在しないので、とりあえずいくつか新しいファイルをコミットしておきましょう。

    wan

と書かれた wan.txt と、

    boo
 
と書かれた、boo.txt を作業ディレクトリに作成し、この二つのファイルを追加したよ、という内容のコミットを作成しましょう。

まずは wan.txt と boo.txt を作成してください。できましたか？ では、git add でこの二つのファイルを stage しましょう。

    $ git status
    # On branch master
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	boo.txt
    #	wan.txt
    nothing added to commit but untracked files present (use "git add" to track)

    $ git add .

ん！？ "git add wan.txt boo.txt" ではなくて、"git add ." としていますね。なんだこれは！？

じつは、 "." というのは、「現在いるディレクトリ」のことを指します。そして、git add などでは、ディレクトリを指定すると、「その中身のファイルを全部」と指定したのと同じになります。つまり、今回で言えば、"git add ." は "git add boo.txt nyan.txt wan.txt"と指定したのと同じことなのです。このとき、nyan.txt にはなんの変更も行われていないため、git は変更があった wan.txt と boo.txt だけを staging area に登録するのです。賢い！

というわけで、今は wan.txt と boo.txt が新規に作成されたよ、という情報が staging area に上がっているはずです。それを見てみましょう。

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	new file:   boo.txt
    #	new file:   wan.txt
    #

よし、意図通りの結果になっていますね！

ではコミットしてしまいましょう。

    $ git commit

エディタが立ち上がるので「犬と豚の鳴き声を追加」とコミットメッセージを書いて保存、終了。

    [master 66c681b] 犬と豚の鳴き声を追加
     2 files changed, 2 insertions(+)
     create mode 100644 boo.txt
     create mode 100644 wan.txt

と出力されました。2ファイルに変更があって、２行追加されているよ、だそうです。1行のファイルを2つ作ったので、計算は合ってますね。

     create mode 100644 boo.txt
     create mode 100644 wan.txt

に関しては、mode 100644 でboo.txtとwan.txtを作ったという変更だよ、ってことですね。「このmode 100644ってのはなんじゃ」って感じですが、Unix系システムの「パーミッション」という概念を知らないと意味不明なので、無視しておいていいです。とりあえず、「boo.txtとwan.txtが追加されたよ」って情報が出ていることを確認してください。

一応 git log も確認しておきましょうか。

    $ git log --graph
    * commit 66c681b9596f28c93bc396320b934cd3cdde46d4
    | Author: Shinpei Maruyama <shinpeim@gmail.com>
    | Date:   Sat May 4 22:05:12 2013 +0900
    | 
    |     犬と豚の鳴き声を追加
    |  
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

うん、いいですね。

と、ここで、唐突に git が怖くなくなる tips をひとつお伝えしましょう。21世紀で、もうあなたのパソコンはいろんな色を表示できるというのに、こんなモノクロの出力ばかり眺めていると気がめいります。あと、やっぱり色がないとわかりずらい。なので git の出力に色を付けちゃいましょう。

    $ git config --global color.ui true 

はい。これで次回から git の出力に色がつきます。

## ファイルの削除

さて、これでようやく「ファイルの削除」の準備が整いました。ディレクターが「俺豚嫌いなんだよね、なくさない？」と言っています。しょうがないので boo.txt を削除してしまいましょう。お好きな手段で作業ディレクトリ内から boo.txt を削除してください。

削除しましたか？

さて、これで、作業ディレクトリ内には、「boo.txtが削除された」という変化がおこりました。git statusでそれを確認してみましょう。

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	deleted:    boo.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

はい、"Changes not staged for commit:" のなかに deleted: boo.txt が記載されています。いいですね。ではこの変更を stage しましょう。

    $ git add boo.txt

確認

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	deleted:    boo.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

アレッ！？Changes not staged for commit: のままですね！？ これはおかしい。

よく git status の出力内容を見てみてください。いままでと少し違う部分がありますね。そうです。括弧の中身が、変わっています。「use "git add/rm <file>..." to update what will be committed」となって、「stage するためには git add/rm を使ってね」と書かれています。

じつは、ファイルの削除という変更を stage するためには、git add ではなく、git rm を使わないとダメなのです。ちょっとめんどうですね。やってみましょう。

    $ git rm boo.txt
    rm 'boo.txt'
    
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	deleted:    boo.txt
    #

うん、きちんと「Changes to be committed」になっています。stage されましたね。

ちなみに、今回は「作業ディレクトリの中からファイルを削除」してから「その変更をstage」しましたが、これをパツイチで行う方法もあります。それを見てみましょう。

そのために、まずは、stage された変更を unstage して、さらに作業ディレクトリで行った変更をなかったことにしましょう。

「use "git reset HEAD <file>..." to unstage」だそうですから、従いましょう

    $ git reset HEAD boo.txt
    D	boo.txt

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	deleted:    boo.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

はい、not staged な状態に戻りました。さらに、作業ディレクトリの変更を「なかったこと」にしましょう。「use "git checkout -- <file>..." to discard changes in working directory」に従えばいいですね。

    $ git checkout -- boo.txt
    
    $ git status
    # On branch master
    nothing to commit, working directory clean

はい、「作業ディレクトリの中の変更点はないよ」状態になっていますね。ディレクトリの中を覗いてみると boo.txt が復活しているのが見て取れると思います。

では、ここから、「作業ディレクトリの中からファイルを削除して、さらにその "削除したよ" という変更をstageする」というのを一気にやってみましょう。

    $ git rm boo.txt
    rm 'boo.txt'

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	deleted:    boo.txt
    #

お、一気に「Changes to be committed:」になりました。つまり、一気に staging area に「削除したよ」という情報を登録するところまでできていますね。念のため、ディレクトリの中を覗いてみてください。boo.txtが無くなっているのが見て取れると思います。

ではこの変更をコミットしましょう。コミットメッセージは「豚の鳴き声は要らないので削除」くらいにしておきましょうか。

    $ git commit
    [master 8faaa1f] 豚の鳴き声は要らないので削除
     1 file changed, 1 deletion(-)
     delete mode 100644 boo.txt

ログを見ます。

    $ git log --graph
    * commit 8faaa1f91278ff3763b44dd79aa884e30b630e62
    | Author: Shinpei Maruyama <shinpeim@gmail.com>
    | Date:   Sat May 4 22:37:54 2013 +0900
    | 
    |     豚の鳴き声は要らないので削除
    |  
    * commit 66c681b9596f28c93bc396320b934cd3cdde46d4
    | Author: Shinpei Maruyama <shinpeim@gmail.com>
    | Date:   Sat May 4 22:05:12 2013 +0900
    | 
    |     犬と豚の鳴き声を追加
    |  
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

うん、良いですね。

まとめです。

* 作業ディレクトリ内で「ファイルの削除」という変更がおこったとき、その変更を stage するためには git add ではなくて git rm を使う。
* 手動で作業ディレクトリ内からファイルを削除しなくても、git rm をいきなり使うと、指定したファイルを作業ディレクトリから削除して、さらにその変更を stage するところまで一気にやってくれる。

といったところです。あっ、ところで、git rm の「rm」ってなんのことでしょうね？ add というのは普通に「加える」でいいのですが。実は、「rm」は remove の略です。これで覚えやすくなりましたね！

## ファイルのリネーム

ディレクターから、「wan.txt とか nyan.txt じゃなくて、もっと英語っぽくならないの」と言われてしまいました。では次はファイルのリネームを行いましょう。どんな手段でもいいので、wan.txt を bow.txtに、nyan.txt を mew.txt にリネームしましょう。

これで「ファイルのリネーム」という変化が作業ディレクトリに起こりましたね。じゃあ git status で確認です。

    $ git status
    # On branch master
    # Changes not staged for commit:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	deleted:    nyan.txt
    #	deleted:    wan.txt
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	bow.txt
    #	mew.txt
    no changes added to commit (use "git add" and/or "git commit -a")
    
「*！？*」なんか出た！怖い！はい、怖くないです。いっこずつ見て行けばなるほどって感じです。

まずは上から見て行きましょうか。

「Changes not staged for commit:」に、deleted として nyan.txt とwan.txt が表示されていますね。つまり、「作業ディレクトリから nyan.txt と wan.txt がなくなってるけど、stage されてないよ」ですね。nyan.txtはどこへ行ってしまったのでしょうか……？ nyan.txt は名前がかわり、mew.txt になりました。これは、別の言い方をすると、「nyan.txt」という名前のファイルは無くなって、同じ内容の「mew.txt」という名前のファイルが出来た、という風にみることができますね。この前半部分 "「nyan.txt」という名前のファイルはなくなって" というのが、この "deleted: nyan.txt" が示していることです。同様に、「wan.txt」という名前のファイルも無くなり、同じ内容の「bow.txt」が生まれています。これで "deleted: nyan.txt" と "deleted: wan.txt" の謎は解けましたね。

では後半の「同じ内容の mew.txt という名前のファイルができた」と「同じ内容の bow.txt という名前のファイルができた」についてみて見ましょう。「Untracked files:」に bow.txt と mew.txt が表示されていますね。つまり、「新しく作業コピーに bow.txt と nyan.txt があるけど、このファイル知らないファイルだよ」と Git さんが言っているわけです。

と言う感じで、「リネーム」というのが「新しい名前で同じ内容のファイルを作って、古いファイルは消す」という動作と同じであるということを理解すれば、「あーなるほどね」という感じで見れるのではないでしょうか。

ではこの変化を stage しましょう。もういいですね。消えたファイルは git rm で stage、増えたファイルは git add で stage です。

    $ git add bow.txt mew.txt
    
    $ git rm nyan.txt wan.txt
    rm 'nyan.txt'
    rm 'wan.txt'
    
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	renamed:    wan.txt -> bow.txt
    #	renamed:    nyan.txt -> mew.txt
    #

おー。staging areaに、renamed: として「wan.txt -> bow.txt」「nayn.txt -> mew.txt」が登録されました。Hooray！

ちなみに、ファイルの削除のときには、「作業ディレクトリからファイルを削除」と「削除したよという情報をstage」を一気に行うことができましたが、リネームのときにはどうでしょうか。実は、リネームのときには、"git mv リネーム前のファイルの名前 リネーム後のファイルの名前" とすることで一気に行えます。今回ならば、

    $ git mv nyan.txt mew.txt
    $ git mv wan.txt bow.txt

とすればOKです。

ではこれを commit しておきましょう。コミットメッセージは「ファイル名を英語に変更」とかでしょうか。

    $ git commit
    [master bb8f610] ファイル名を英語に変更
     2 files changed, 0 insertions(+), 0 deletions(-)
     rename wan.txt => bow.txt (100%)
     rename nyan.txt => mew.txt (100%)

出ている情報も問題ないですね。最後に git log で確認。

```
* commit bb8f61009a30190c1bda537ae7f9c40ab22f2e62
| Author: Shinpei Maruyama <shinpeim@gmail.com>
| Date:   Sat May 4 23:00:48 2013 +0900
| 
|     ファイル名を英語に変更
|  
* commit 8faaa1f91278ff3763b44dd79aa884e30b630e62
| Author: Shinpei Maruyama <shinpeim@gmail.com>
| Date:   Sat May 4 22:37:54 2013 +0900
| 
|     豚の鳴き声は要らないので削除
|  
* commit 66c681b9596f28c93bc396320b934cd3cdde46d4
| Author: Shinpei Maruyama <shinpeim@gmail.com>
| Date:   Sat May 4 22:05:12 2013 +0900
| 
|     犬と豚の鳴き声を追加
|  
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
```

うん、OKです。

## ファイルの移動

「動物のファイル、animals ディレクトリにまとめたいね」という話が出てきました。animalsディレクトリを作って、ファイルをそこに移動させましょう。例によって手段はなんでもいいのでやってみてください。

移動させましたか？では git status。

    # On branch master
    # Changes not staged for commit:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	deleted:    bow.txt
    #	deleted:    mew.txt
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	animals/
    no changes added to commit (use "git add" and/or "git commit -a")
    
はい、もう怖くないですね。ファイルの移動というのはファイルのリネームと同じく、「新しい場所に同じ内容のファイルを作成して、古いファイルは消す」と一緒です。なので bow.txt と mew.txt が「削除されてるけどその変更は stage されてないよ」と表示され、新しく作られた animals/ ディレクトリが「あたらしくディレクトリできてるけどこれ知らないディレクトリだよ」と言われています。

では新しいディレクトリに対する変更を add で stage して、古いファイルの削除という変更を rm で stage しましょう。

    $ git add animals

    $ git rm bow.txt mew.txt
    rm 'bow.txt'
    rm 'mew.txt'

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	renamed:    bow.txt -> animals/bow.txt
    #	renamed:    mew.txt -> animals/mew.txt
    #
    
おっと？ Changes to be committed つまり stage された変更の中に、"renamed" と表示されました。でもこれも少し考えれば納得ですね。さきほど、"ファイルの移動というのはファイルのリネームと同じく、「新しい場所に同じ内容のファイルを作成して、古いファイルは消す」と一緒です" と言ったのを思い出してください。リネームも移動も、どちらもコンピューターから見れば、「新しい場所に同じ内容のファイルを作成して、古いやつは消す」という操作になっているのです。なので、リネームと移動を区別する必要はないのですね。

ということは？ そうです。どうぜん、 $ git mv を使えば、ファイルの移動とその変更内容をstageするのを一気にやってしまえます。
今回で言えば、

    $ mkdir animals
    $ git mv bow.txt animals/bow.txt
    $ git mv mew.txt animals/mew.txt

で良いですね。最初に animals ディレクトリを作成しないとそもそもファイルの移動ができないので、最初に animals ディレクトリを作っているところに注意してください。

ではコミット。コミットメッセージは「動物ファイルを animals ディレクトリに移動」くらいでしょうか

    $ git commit
    [master 5fe17c7] 動物ファイルを animals ディレクトリに移動
     2 files changed, 0 insertions(+), 0 deletions(-)
     rename bow.txt => animals/bow.txt (100%)
     rename mew.txt => animals/mew.txt (100%)
     
ログを確認

     * commit 5fe17c7e3f0b2252b4081d08778fdfcbd232e7d9
     | Author: Shinpei Maruyama <shinpeim@gmail.com>
     | Date:   Sat May 4 23:20:06 2013 +0900
     | 
     |     動物ファイルを animals ディレクトリに移動
     |  
     * commit bb8f61009a30190c1bda537ae7f9c40ab22f2e62
     | Author: Shinpei Maruyama <shinpeim@gmail.com>
     | Date:   Sat May 4 23:00:48 2013 +0900
     | 
     |     ファイル名を英語に変更
     |  
     * commit 8faaa1f91278ff3763b44dd79aa884e30b630e62
     | Author: Shinpei Maruyama <shinpeim@gmail.com>
     | Date:   Sat May 4 22:37:54 2013 +0900
     | 
     |     豚の鳴き声は要らないので削除
     |  
     * commit 66c681b9596f28c93bc396320b934cd3cdde46d4
     | Author: Shinpei Maruyama <shinpeim@gmail.com>
     | Date:   Sat May 4 22:05:12 2013 +0900
     | 
     |     犬と豚の鳴き声を追加
     |  
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
           
はい、OKです。

まとめましょう。

* 作業ディレクトリ内での「ファイルのリネーム、移動」は、実質は「新しい場所(名前)に同じ内容のファイルを作って、古い場所にあるファイルを消す」という動作である。
* なので、ファイルをリネーム/移動 した場合は、「新しくファイルができたよ」という情報を git add で stage、「古いファイルは消えたよ」という情報を git rm で stage することでリネーム/削除が表現できる。
* git mv 使うと、「作業ディレクトリ内のファイルのリネーム/移動」と、「その変更を stage」を一気にやってくれる

ちなみに、git mv の mv は move の略です。

次回はブランチについて見て行きましょう。