# みんなでつかう - push pull

さて、前回までで、「たかしのリポジトリと作業ディレクトリ」「ゆうすけのリポジトリと作業ディレクトリ」「みんなで触るリポジトリ」の準備ができました。今回はここから協調作業をおこなっていきましょう。

## ゆうすけ、頑張る

あなたはゆうすけです。ゆうすけは、ディレクターから例の「文体変更」の作業を頼まれていましたね。ではゆうすけの作業ディレクトリでその作業を行って生きましょう。

    $ cd path/to/yuusukes_workspace
    $ git checkout -b feature/unify_styles development

おや、いくつか新しい感じのものが出てきました。 git checkout -b は「ブランチを切ってそれをチェックアウト」ですね。今回は「feature/unify_styles」という名前のブランチを切っていますね。これは、「このブランチはフィーチャーブランチだよ」ということをわかりやすくするために、こういう名前を付けただけです。しかし、そのあと、に development という指定をさらにしてあります。

これはどういうことかと言うと、新しいブランチを「どこから」切るのかを指定したのです。

おそらく、いまゆうすけであるあなたは master ブランチを選択していたでしょう。この状態で「git branch -b ＜新しく作るブランチの名前＞」とすると、master ブランチから分岐するブランチが切られます。しかし、「git branch -b ＜新しく作るブランチの名前＞ ＜分岐元＞」とすることで、明示的に「どこからブランチを分岐させるのか」を指定することができるのです。

さて、これで新しいフィーチャーブランチを切ってチェックアウトしたので、ここで作業を行いましょう。

* cat_lover_said.txt

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っており、また人間も
人間で猫に使役されることを至上の喜びと思う傾向にある。

猫の魅力に取り付かれた人間は二度と猫の魅力から離れることが
できないとも言われている。

猫というのはかように本当にかわいいものである。

猫好きではない人間は、頭がおかしいのではないだろうか。
```

さて、これで文体を統一しました。ではこの変更をコミットしましょう。コミットメッセージは「文体を統一」でいいでしょう。

## たかし、頑張る

さて、ゆうすけが文体の統一という作業を行っている間に、たかしにはディレクターから「頭おかしい」を急いで直せという指示が飛んでいました。

あなたは今からたかしです。たかし、がんばりましょう。

たかしの作業ディレクトリに移動し、マスターブランチから hotfix ブランチを切り、チェックアウトします。

    $ cd path/to/takashis_workspace
    $ git checkout -b hotfix master
    
そして緊急で「頭おかしい」を直しましょう。

* cat_lover_said.txt

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っており、また人間も
人間で猫に使役されることを至上の喜びと思う傾向にある。

猫の魅力に取り付かれた人間は二度と猫の魅力から離れることが
できないとも言われており、猫ってほんとうにかわいいですね。

もうほんとうに可愛い、猫。
猫。
猫が本当にかわいい。

猫好きじゃない人間がいるのが不思議
```

はい、書き換えましたね。ではこれをコミット。メッセージは「頭おかしいという表現はまずいので修正」です。

そして、この修正で OK かどうかをディレクターに見てもらうために、この「hotfix」というブランチを「みんなで触るリポジトリ」に反映させましょう！

## たかし、固まる

さて、手元のリポジトリの変更をリモートリポジトリに反映させるためにはどうするのでしたっけ？ そうです、追跡ブランチですね。今回ならば、origin/hotfix を追跡するブランチを作れば…………、とここまで考えて、たかしは「アレっ」となりました。そもそもorigin/hotfix というリモートブランチが存在しない……。

こういうときには、「リモートリポジトリに新しいブランチを複製」してしましましょう。方法は、"$ git push ＜リモートリポジトリの名前＞ ＜手元のブランチ＞:＜リモートに作りたいブランチ＞" です。

    $ git push origin hotfix:hotfix
    Counting objects: 5, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 371 bytes, done.
    Total 3 (delta 1), reused 0 (delta 0)
    To ../shared_repo.git
     * [new branch]      hotfix -> hotfix

いろいろ出てきました。なんかいろいろ出ましたが、重要なのは最後の 2 行です。「shred_repo.git に対して: ・新しいブランチ  hotfix -> hotfix」という感じですね。うん。みんなで触るリポジトリにhotfixを作成することができました。

ちなみに、今回のように、手元のリポジトリでのブランチの名前とリモートリポジトリのブランチの名前が同一の場合は、

    $ git push <リモートリポジトリの名前＞ ＜ブランチの名前＞

でも OK です。

さて、これで「みんなで触るリポジトリ」にhotfixブランチを作成することができました。じゃあ、手元の hotfix ブランチがこのリモートブランチを追跡するように設定しておきましょう。

    $ git branch --set-upstream-to=origin/hotfix hotfix

はい、いいですね。

## たかし、マージする

さて、「みんなで触るリポジトリ」に hotfix ブランチを push したたかしは、ディレクターに「みんなで触るリポジトリのhotfixブランチの内容をチェックしてください。OK ならばマスターに merge して修正リリースとします」と説明しました。ディレクターはそれを聞いて内容をチェックして、「よし、OK」と OK を出しました。OK を出されたので、これを共有リポジトリの master にマージしたいですね。

しかし、共有リポジトリを直接いじることはできません。そういうときにはどうするのでしたか？ そうです、「追跡ブランチ」を変更すればいいのでしたね。今、共有リポジトリの master ブランチを追跡しているのは、手元の master ブランチです。

では、hotfix の内容を手元の master ブランチに merge しましょう。これは簡単ですね。

    $ git checkout master
    $ gir git merge --no-ff hotfix 

です。では現在のグラフを確認しましょう。

```
$ git graph
*   38f43dc  (HEAD, master) 2013-05-07 Shinpei Maruyama Merge branch 'hotfix'
|\  
| * 1d83043  (origin/hotfix, hotfix) 2013-05-07 Shinpei Maruyama 頭おかしいという表現はまずいので修正
|/  
* a10bcb5  (origin/master, origin/development, development) 2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

手元の master ブランチは、できたてのマージコミットを指しています。しかし、origin/master はまだこのマージコミットを指していませんね。それもそのはず、そもそもまだこのコミットはたかしの手元のリポジトリにしか存在しません。

では、ここで git status を確認してみましょう。

```
$ git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#   (use "git push" to publish your local commits)
#
nothing to commit, working directory clean
```

お、初めて見る表示が出てきました。なになに？

「君のブランチは origin/master よりも 2 コミット進んでいるぜ！(もし君の手元のコミットを公開してしまいたければ、 git push するといいぜ！)」だそうです。また Git さんのキャラがブレていますが、Git さんのキャラがぶれてるときは筆者が疲れている証拠です。暖かく見守って下さい。

## たかし、push する

では、Gitさんに言われたとおり、ここで git push で共有リポジトリにこの変更を公開しましょう！

```
$ git push
warning: push.default is unset; its implicit value is changing in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the current behavior after the default changes, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

Counting objects: 1, done.
Writing objects: 100% (1/1), 226 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To ../shared_repo.git
   a10bcb5..38f43dc  master -> master
```

ヒャー！なんか warning が出た！

出なかったひとは Git のヴァージョンを上げましょう。あなたの使っている Git はすでに古いです。

では、warning の内容を読んでみましょう。

```
警告： push.default が設定されてないよ。push.defaultが設定されていないとき
の値は、Git 2.0 で「matching から simple」に変更されるよ。
以下のようなコマンドを打つと、今後このメッセージは出なくなるし、
変更されてからも今までと同じ動きをするよ

  git config --global push.default matching

以下のようなコマンドを打つと、今後このメッセージは出なくなるし
2.0以降の新しい動きをするようになるよ。

  git config --global push.default simple

もっと詳しい説明が聞きたければ、'git help config' と打って、
'push .default'の部分を探してくれよな。
(ちなみに'simple'モードは Git 1.7.11 から使えるんだ。もし古い Git の
ヴァージョンを使うようなら、似たような 'current' ってモードを使ってくれ)
```

だそうです。

'simple'？ 'matching'？ 'push.default'？ よくわからないですね。説明します。

この push.default というのは、「git push」とコマンドを実行したときに、Git さんがどのような動きをするのかを設定する設定項目です。'matching' モードだと、今選択されているブランチがどのブランチかには関係なく、「手元にあるすべての追跡ブランチの変更をリモートに反映する」という動きになります。'simple'モードは、「今選択されているブランチが追跡ブランチであれば、手元のブランチの変更内容をリモートに反映する」という動きになります。

では、その下の表示も見てみましょう。

3 行ログが表示されて、その下に、 shared_repo.git に対して master -> master したよ、と書かれています。うん、手元の master からリモートの master に変更が反映されたみたいですね！

ん？ちょっとまってください。現在は、git push は「matching」モードになっているはずです。ならば、development ブランチや hotfix ブランチもリモートに反映されないとおかしいのでは？

そのとおりです。でも、よく考えてみると、development ブランチも hotfix ブランチも、手元とリモートの間に差異がありませんね。なので、Gitさんは、「じゃあ反映させるのは master ブランチだけでいいか」と判断して、masterブランチだけが反映されたのです。

と、今回 git push で行われた内容を一通り確認したところで、push.default の値をどちらに設定するか、決めておきましょう。まあ、ここは素直に新しいほうに合わせて simple に設定しておきましょう。こっちのほうが「あっやべ！ push しちゃいけないやつ push しちゃった！」みたいな事故にあわずに済みそうです。

さて、それでは現在のグラフを確認しましょう。

```
$ git graph
*   38f43dc  (HEAD, origin/master, master) 2013-05-07 Shinpei Maruyama Merge branch 'hotfix'
|\  
| * 1d83043  (origin/hotfix, hotfix) 2013-05-07 Shinpei Maruyama 頭おかしいという表現はまずいので修正
|/  
* a10bcb5  (origin/development, development) 2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

うん、origin/master もマージコミットを指すようになりましたね。

## たかし、あと始末をする

さて、これで無事に共有リポジトリの master ブランチに緊急リリースのためのコミットが生まれました。さっさとリリースしてしまいましょう。

無事にリリースされたそうです。では、あと始末をしましょう。今からやらなければならないのは大まかに以下の二つです。

* hotfix ブランチはもう用無しなので、手元からもリモートからも消してしまう
* 緊急リリースによる変更内容を、開発ブランチにも反映させる

さあ、ではやっていきましょう。手元の hotfix ブランチを消すのは簡単ですね

    $ git branch -d hotfix

ではリモートのhotfixブランチも消しましょう。ええと、origin/hotfix を消すという変更だから、そのための追跡ブランチは…………あれ、今手元で消しちゃった。消したっていう内容を反映するために追跡ブランチが必要なんだけど消したっていう内容を生むためには消す必要があって push できない……。という感じですね。そんなわけなんで、リモートのブランチを消すためにはそのためのコマンドを別に打つ必要があります。

    $ git push origin :hotfix
    
です。「はぁ〜〜〜〜！？なにこのコマンド、このコマンドのどこに「消す」っていう情報が含まれてるの、なんなのこの直感的でないコマンド、ふざけてんのか。」……と思う気持ちもわかります。これ、わかりにくいですね。でも、リモートにブランチを作ったときの git push の構文をもう一度思い出してください。

    $ git push ＜リモートリポジトリの名前＞ ＜手元のブランチ＞:＜リモートに作るブランチ＞

でしたね。これと上のコマンドを見比べてみると、

    $ git push ＜リモートリポジトリの名前＞ (ここになにも書いてない):＜リモートのブランチ＞

となっています。つまり、「なにもなし」で＜リモートのブランチ＞を上書きしてるみたいな感じですね。なんかプーさんの「なにもしないをしてるんだよ」に似た納得できなさも感じますが、まあそういうものだと思えば、少しは覚えやすいのではないでしょうか。

なにはともあれ、これで hotfix ブランチを手元でもリモートでも削除することに成功しました。

では、次は、 hotfix で対応された内容が反映されている master を、 development に取り込みましょう。

    $ git checkout development
    $ git mrege --no-ff master

コミットメッセージは、なんのためにmasterをマージしたのかわかりやすいように「緊急対応で行った修正をmasterから取り込む」くらいにしておきましょうか。

はい、これで手元の development の準備ができましたね。それではここでリモートにこれを反映しましょう。

    $ git push

たかしくん、おつかれさまでした。

## 一方ゆうすけは

さて、ではあなたはゆうすけの役に戻りましょう。ゆうすけ、今どんな状態でしたっけ？ ゆうすけの作業ディレクトリに行って、状態を確認しましょう。

```
$ cd path/to/yuusukes_workspace
$ git graph
* 3422d74  (HEAD, feature/unify_styles) 2013-05-07 Shinpei Maruyama 文体を統一
* a10bcb5  (origin/master, origin/development, origin/HEAD, master, development) 2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

ああ、そうでした。文体を統一する作業を終えて、手元のリポジトリにコミットしたところでしたね。では、この内容をディレクターにチェックしてもらうために、共有リポジトリに push しましょう。

共有リポジトリにはまだ feature/unify_styles が存在しないので、共有リポジトリにこのブランチをコピーしないとだめですね。

```
$ git push origin feature/unify_styles
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 922 bytes, done.
Total 6 (delta 1), reused 0 (delta 0)
To /Users/shinpeim/shared_repo.git
 * [new branch]      feature/unify_styles -> feature/unify_styles
```

うん、いいでしょう。これでディレクターにチェックしてもらいましょう。

はい、ディレクターから OK が出ました。最新の開発版にこれを反映させるために、development ブランチにマージしたいですね。

でもちょっとまって！共有リポジトリの development ブランチには、すでにたかしが変更を加えていました。なので、さきにそのたかしが行った変更をゆうすけは手元に持ってきておきたいですね。

リモートリポジトリのコミットやブランチを手元にコピーしてくるコマンドは覚えていますか？そうです。 git fetch です。

```
$ git fetch origin
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 3), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
From /Users/shinpeim/shared_repo
   a10bcb5..c5e394e  development -> origin/development
   a10bcb5..38f43dc  master     -> origin/master
```

はい、これでリモートリポジトリの内容がコピーされてきました。リモートリポジトリのブランチは「リモートブランチ」としてコピーされてくるのでしたね。いいですか？

ではここでグラフを確認してみましょう。

```
$ git graph
*   c5e394e  (origin/development) 2013-05-07 Shinpei Maruyama 緊急対応で行った修正をmasterから取り込む
|\  
| *   38f43dc  (origin/master, origin/HEAD) 2013-05-07 Shinpei Maruyama Merge branch 'hotfix'
| |\  
|/ /  
| * 1d83043  2013-05-07 Shinpei Maruyama 頭おかしいという表現はまずいので修正
|/  
| * 3422d74  (HEAD, origin/feature/unify_styles, feature/unify_styles) 2013-05-07 Shinpei Maruyama 文体を統一
|/  
* a10bcb5  (master, development) 2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

見事にリモートリポジトリのコミットとブランチが取り込まれています。リモートブランチと手元のブランチを比べてみると、手元のブランチはリモートリポジトリよりもだいぶ「昔のコミット」を指しています。これをぜひともリモートブランチのところまで進めてしまいたいですね。そうです、「Fast-forward」したいのです。じゃあそうしましょう。

```
$ git checkout master
$ git mrege origin/master
$ git checkout development
$ git merge origin/development
```

はい、これで Fast-forward されて手元のブランチがリモートリポジトリに「追いつき」ました。グラフを確認しましょう。

```
$ git graph
*   c5e394e  (HEAD, origin/development, development) 2013-05-07 Shinpei Maruyama 緊急対応で行った修正をmasterから取り込む
|\  
| *   38f43dc  (origin/master, origin/HEAD, master) 2013-05-07 Shinpei Maruyama Merge branch 'hotfix'
| |\  
|/ /  
| * 1d83043  2013-05-07 Shinpei Maruyama 頭おかしいという表現はまずいので修正
|/  
| * 3422d74  (origin/feature/unify_styles, feature/unify_styles) 2013-05-07 Shinpei Maruyama 文体を統一
|/  
* a10bcb5  2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

うん、きちんと追いついています。

さて、いちいちこうやってリモートリポジトリの内容を取得してきて、手動で merge するのは正直だるいですね。だるすぎです。そのために用意されているの git pull です。

git pull は、fetch からマージを自動でやってくれます。普段はこちらを使うと良いでしょう。ただし、気をつけてください。さきほど "git pull は fetch からマージを自動でやってくれる" といいました。そうです、「マージ」を行うのです。そのため、git pullを行うと「コンフリクト」が起こる可能性もあります。でも、もうあなたはコンフリクトがおこったおきにどうすればいいか、知っていますね。なら怖がらなくても大丈夫。落ち着いて競合を直し手動でマージコミットを作ればいいのです。

当然、このマージコミットは「手元」にしかないマージコミットなので、適切に共有リポジトリに push する必要も出てくるでしょう。でも、もうあなたはリモートリポジトリと手元のリポジトリの関係を適切にハンドルすることができるようになっています。これもなにも怖くないですね。

## ゆうすけ、development にマージする

さて、これでゆうすけのリポジトリは「みんなで触るリポジトリ」に「追いつき」ました。再度グラフを確認しましょう。

```
$ git graph
*   c5e394e  (HEAD, origin/development, development) 2013-05-07 Shinpei Maruyama 緊急対応で行った修正をmasterから取り込む
|\  
| *   38f43dc  (origin/master, origin/HEAD, master) 2013-05-07 Shinpei Maruyama Merge branch 'hotfix'
| |\  
|/ /  
| * 1d83043  2013-05-07 Shinpei Maruyama 頭おかしいという表現はまずいので修正
|/  
| * 3422d74  (origin/feature/unify_styles, feature/unify_styles) 2013-05-07 Shinpei Maruyama 文体を統一
|/  
* a10bcb5  2013-05-07 Shinpei Maruyama 猫好きの話を追加
```

さて、今から何を行うのでしたっけ？ ああ、そうでした、feature/unify_styles の変更を development に反映したいのでした。ではやってしまいましょう。

```
$ git checkout development
$ git merge feature/unify_styles
Auto-merging cat_lover_said.txt
CONFLICT (content): Merge conflict in cat_lover_said.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Oops！コンフリクトです。そういえば、hotfix でも unify_styles でも、一番最後の「頭おかしい」の行を修正しているのでした。コンフリクトを解決しましょう。コンフリクトしたファイルを手動で修正して、git add、 git commit でしたね。

今回はコミットメッセージに「なぜコンフリクトが起こったのか、どうコンフリクトを解決したのか」も書いておきましょう。「コンフリクトの修正<改行><改行>文体を修正するための修正と、"頭おかしい" の修正で同じ行を修正していたので、どちらの修正も生かすような対応を行った」くらいですかね。コミットの内容を詳しく書きたいときには、こんな感じでコミットメッセージの一行目にコミットの概要を書き、ふたつ改行を行ったあとに詳細を書くと、いろんなツールでコミットログを見たときにいい感じに表示してくれるので、こういう書き方をするといいでしょう。

はい、これで無事にマージが行われました。ではこれを共有リポジトリにpushしましょう。

    $ git push

はい、無事に push できました。おつかれさまでした！

あとはレビューしてもらって、OK ならばmasterに反映して「新規リリースバージョン」をリリースすればいいですね！

## git push 落ち穂ひろい

さて、今回は何事もなくスムースにことが運びましたが、git push が成功するためには、条件があります。

* リモートリポジトリが、bare リポジトリである
  * 覚えてますか？bare リポジトリ。じつはbareリポジトリへじゃないと、pushは受け入れてもらえません。自分が作業してるところにどっかからpushされてきたら混乱しちゃいますよね。
* リモートリポジトリへの書き込み権限を持っている
  * 今回は同じファイルシステム上の書き込み権のあるリポジトリだったから push 可能だったけれど、たとえば github の他人のリポジトリだとか、そういう「書き込み権」のないリポジトリに対して push しても書き込みはできません。まあ、当然ですね。
* 手元のリモートブランチがリモートリポジトリに「追いついて」いる
  * 自分の手元にあるコミットよりもリモートのコミットのほうが「先に」進んでいたら、そのコミットは「あれ。親コミットが違う……」これどうすればいいんだろう……となってしまいます。このような場合には、適宜 git fetch からの merge や git pull を行って(その際の merge ではコンフリクトが発生する可能性があります)、手元のリモートブランチをリモートリポジトリに追いつかせてからpushしてください。
* すでにリモートリポジトリに push 済みのコミットが、手元のリポジトリで改変されていない
  * push 済みのコミットが手元で改変されてしまうと、リモートリポジトリと手元のリポジトリの間で不整合が起こってしまいます。

さて、では今回のまとめです。

* リモートリポジトリに手元の変更を反映したいなら、git push
  * 追跡ブランチに対しての変更はそのまま git push
  * 新しいブランチを作りたいなら git push ＜リモートリポジトリの名前＞ ＜手元のブランチ＞:＜リモートのブランチの名前＞
  * ブランチを削除したいなら git push ＜リモートリポジトリの名前＞ :＜リモートのブランチの名前＞
* リモートリポジトリから手元に変更を持ってきたいなら git fetch
  * リモートリポジトリのコミットとブランチを持ってくる
  * リモートリポジトリのブランチはリモートブランチとして作られる
* fetch のついでに merge も一緒にやりたいなら、git pull

これで、ひととおりこのドキュメントはおしましです。たいへんおつかれさまでした。ここまで丁寧に読んでくれたあなたが、今日から快適なヴァージョン管理ライフを過ごせることを祈っています。

## 最後に

さて、このドキュメント自体も git リポジトリで管理されています。が、このリポジトリにはなんと master ブランチしか存在しません。ひどいですね。ただ、すこし言い訳をしておくと、あくまで Git は道具です。わたしは今回このドキュメントを書くにあたり、「ブランチは一本でいいや、なにか大きな改修を行うときになったらブランチを切ろう」というポリシーで書くことに決めました。今回はけっこうきちんとしたブランチ運用を見てみましたが、もちろん、全てのプロジェクトでこのような運用をする必要はありません。あるいは、もっときちんとした運用をするべきプロジェクトもあるでしょう。

大切なのは、ベストプラクティスを知っていて、必要な場所で必要なときにいつでも使えるようになることです。あるひとつのやり方に固執する必要はありません。そのときに一番良いやりかたを選択していきましょう。

そのために、いろいろなブランチの運用方法を知っておくことは大切なことです。本書は、そのためのポインターをふたつだけ最後に書いて、おしまいとしましょう。

* [見えないチカラ: A successful Git branching model を翻訳しました](http://keijinsonyaban.blogspot.jp/2010/10/successful-git-branching-model.html)
* [GitHub Flow (Japanese translation)](https://gist.github.com/Gab-km/3705015)

## もうちょっとだけ続くんじゃ

あ、そうそう、これで本編はおしまいですが、この文章で出てきたコマンドを逆引きできる逆引きコマンド集を付録として付けます。もしよかったら、慣れるまではそちらを手元に置いて作業をするといいかもしれないですね。

では、今度こそ。おつかれさまでした！