# ひとりでつかう - もっとマージ

さて、前回までで、ブランチを使いこなしてはじめてのマージを行うところまでやってみましたね。では今回は、もっと実践的なマージを行ってみましょう。

## まずは準備から

今回は、前回まで使っていた「my_first_workspace」とはまたべつの作業ディレクトリとリポジトリを作成して、そこで作業をおこないましょう。まずは、my_second_workspace というディレクトリを作成し、そこにリポジトリを作成してください。

    $ mkdir my_second_workspace
    $ cd my_second_workspace
    $ git init

はい、よいですね。

では、まずはここに以下のようなふたつのファイルを作成しましょう。

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

猫好きじゃない人間とか頭がおかしいとしか思えない。
```

* cat_hater_said.txt

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っているが、冗談じゃない。
人間が猫を飼ってやっているのである。そこを勘違いする猫は
本当にダメである。

その点犬は良い。犬は人間のことを「ご主人様」だとおもって
なついてくれる。それならばこそかわいがる気も起きるというものである。

犬は賢い。犬がいい。犬かわいい。散歩に行って息が上がった
あの「ヘッヘッヘッヘッへ」がもう私をおかしくしちゃうほんとにかわいい。

犬大好き。犬最高。

猫好きな人間とか頭がおかしいとしか思えない。
```

いろいろひどいですが、ひとまずこれをコミットしましょう。

    $ git add .
    $ git commit

コミットメッセージは「猫好きの話と犬好きの話を作成」くらいでいいでしょう。

これで準備は完了です。

## 編集作業はブランチで

さて、ディレクターさんから変更の指示が入ってきました。「興奮しすぎて文体が途中で変わっちゃってるじゃん、これじゃだめだよ、直して。」だそうです。

Git 運用のコツとして、なにか変更を行うときにはブランチを作ってそちらのブランチで作業するとよい、というのがあります。それがなぜかは、後で見てみることとして、まずはじゃあ作業用のブランチを切りましょう。ブランチを作成することを、ギョーカイ用語(？)で「ブランチを切る」と言ったりします。文体を統一するためのブランチなので、「unify_styles」という名前のブランチにしましょうか。ブランチを切ってそのブランチを checkout します。

    $ git branch unify_styles
    $ git checkout unify_styles

でいいですね。

念のため今の状態を確認しておきましょう。

    $ git graph
    * 8efecbd  (HEAD, unify_styles, master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
    
    $ git branch
      master
    * unify_styles

はい。最初のコミットがあって、unify_styles ブランチも master ブランチもそのコミットを指していますね。 `git branch` の結果を見ると、きちんと unify_styles が選択されています。

では、ここで文体を直して行きましょう。直した結果は下のようになりました。

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

* cat_hater_said.txt

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っているが、冗談じゃない。
人間が猫を飼ってやっているのである。そこを勘違いする猫は
本当にダメである。

その点犬は良い。犬は人間のことを「ご主人様」だとおもって
なついてくれる。それならばこそかわいがる気も起きるというものである。

犬は賢い。やはり犬がよい。犬はかわいい。

散歩に行ったあとに息が上がっている様など、ほんとうに良いものである。

猫が好きな人間は、頭がおかしいのではないだろうか。
```

はい、修正しました。これをコミットしましょう。

    $ git add .
    $ git commit

コミットメッセージは「文体を統一」でいいでしょう。

ではここで状態を確認です。

```
$ git graph
* ff00bb6  (HEAD, unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
* 8efecbd  (master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

うん、いいですね。新しいコミットが作られ、unify_styles ブランチはその新しいコミットを指しています。

さて、ひとまずこれで、あなたはディレクターにこの修正を見せました。すると、ディレクターは「今忙しいからチェックするまで 2 週間かかるわ」と返事をして、別の仕事にとりかかってしまいました。えー、せっかく急いでやったのに……。

# hotfix もブランチで

さて、そんな折、ディレクターが血相を変えてやってきました。「"頭がおかしい" って表現まずいよ！ 怒られちゃったよ！ すぐ直して！ 文体直したやつはまだチェックしてないから、そのバージョン出すにはあと2週間かかるけど、以前の状態から「頭おかしい」って表現だけ直したやつでいいからすぐ出して！ ていうかなんでこんな表現使ってんだお前頭おかしいのか！」返す言葉もないですね。

VCS を使っていなければ「えー前の状態に戻してからまた別の編集するの……まじで……だるい……」という感じですが、あなたには Git さんがついています。Git の力を借りて、「復元」して、そこから「分岐」した編集を作り出しましょう。

なにはともあれログを確認

```
$ git graph
* ff00bb6  (HEAD, unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
* 8efecbd  (master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

masterのところを復元して、そこから編集をすればよさそうですね。やりましょう。

```
$ git checkout master
```

これで master が指すコミットの状態が作業ディレクトリ内に復元され、HEAD が 指すブランチが master になったはずです。確認しましょう。まずはファイルを開いて、ファイルの中身が復元されていることを確認してください。

確認しましたか？ きちんと復元されていますね。では今度は graph の確認です。

```
$ git graph
* ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
* 8efecbd  (HEAD, master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

HEAD が masterを指している(= masterブランチが選択されている)のが確認できました。

ではここから、緊急の作業のためのブランチを切ってそこに移動しましょう。名前は hotfix にしましょうか。今までは

```
$ git branch ブランチ名
$ git checkout ブランチ名
```

とふたつのコマンドを打っていましたが、「ブランチを切ってそこに移動する」というのはじつは一発でできるコマンドがあります。今回はそちらを使いましょう。

```
$ git checkout -b hotfix
``` 

これで、 hotfix という名前のブランチを作成し、そのブランチを選択したことになります。確認してみましょう。

```
$ git branch
* hotfix
  master
  unify_styles
```

hotfixができていて、選択されていますね。graph も確認します。

```
$ git graph
* ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
* 8efecbd  (HEAD, master, hotfix) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

hotfix は master と同じコミットを指していますね。

では、この hotfix 上で緊急の編集を行いましょう。

「猫好きじゃない人間とか頭がおかしいとしか思えない。」を「猫好きじゃない人間がいるのが不思議。」に書き換え、「猫好きな人間とか頭がおかしいとしか思えない。」を「猫好きな人間がいるのが不思議。」に書き換えましょう。

書き換えましたか？

ではそれをコミットしてください。コミットメッセージは「頭がおかしいという表現はまずいので修正」あたりでいいでしょう。

ではここで graph 確認。

```
* 5f26eb2  (HEAD, hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
| * ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  (master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

はい、いいですね。文体を統一するための変更とは別のルートに、緊急対応で "頭がおかしい" という表現を直した状態のファイルがコミットできました。

さて、できあがったので、あなたはこのブランチでの変更内容をディレクターに見せに行きました。「OK!それでリリースしちゃって！」との言葉をもらったので、この hotfix ブランチで行った変更をリリースするために、master ブランチに取り込みましょう。

## hotfixをマージしよう

あるブランチで行った変更を別のブランチに取り込むためには、 merge でしたね。今回は、master ブランチに hotfix ブランチの内容を取り込みたいので、 master ブランチから merge を行いましょう。

```
$ git checkout master
$ git merge hotfix
Updating 8efecbd..5f26eb2
Fast-forward
 cat_hater_said.txt | 2 +-
 cat_lover_said.txt | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)
```

アレっ！？ おかしいですね。 git merge をすると、マージコミットのためのコミットメッセージを書くためにエディタが立ち上がるはずなのに、何も立ち上がらずマージが行われてしまいました。そして、この前 'recursive' strategy がどうこうと表示されていたところに「Fast-forward」と書かれています。なんだこれは？ とりあえず、ログを確認しましょう。

```
$ git graph
* 5f26eb2  (HEAD, master, hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
| * ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

！？ マージコミットが作成されていません。なんだこれは！？

でも、よく見てみてください。master ブランチが指すコミットが、先ほどまでと変わっているのがわかりますか？

merge する前は、「猫好きの話と犬好きの話を作成」のコミットを指していた master ブランチが、今は hotfix と同じコミットを指しています。なぜこんなことがおこったのでしょうか。

それを理解するために、今はマージの登場人物である hotfix ブランチと master ブランチだけに注目して見てみましょう。

merge を行う前のグラフを再度見てみましょう。

```
* 5f26eb2  (HEAD, hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
| * ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  (master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

でしたね。今回のマージには関係のない unify_styles を無視してこのグラフを見てみましょう。

```
* 5f26eb2  (HEAD, hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
|  
* 8efecbd  (master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

となっています。このグラフ、一本道で、「分岐」してませんね。さて、このとき、master ブランチが hotfix ブランチの変更内容を取り込もうと思った場合、一番楽な方法はなんでしょうか。単純に master ブランチを hotfix と同じところを指すようにしてしまえば、それで master ブランチに hotfix ブランチの変更内容を反映したのと同じことになりますね。そういうとき、賢い Git さんは「じゃあそれでええやん」という感じで単純に master のブランチを hotfix と同じところまで進めるのです。で、こういう merge の仕方を「Fast-forward」と呼んでいるのです。

へー。

これはこれで便利なのですが、わたしはこの挙動があまり気に入りません。だって、このログからは「このタイミングで hotfix ブランチをマージしたよ」という情報が消えてしまっています。マージ作業だって立派な「変更履歴」なのに、それがログに残らないのは気分が悪いです。Fast-forward なんてよけいなお世話だ！というわけで、Fast-forward できるときにも Fast-forward せず、いつもどおりマージコミットを作成することはできないのでしょうか？じつはできるのです。実際にやってみましょう。

おっと、でも、そのためには、さっきの merge で hotfix と同じところまで進んでしまった master ブランチを、元の位置に戻さないとだめですね。 `git reset --hard ＜戻りたいコミットのid＞` というコマンドで、今選択しているブランチをそこまで戻し、作業ディレクトリ内にそのときの状態が復元することができます。今回なら、"8efecbd" のコミットに戻りたいので、それを指定しましょう。

    $ git reset --hard 8efecbd
    HEAD is now at 8efecbd 猫好きの話と犬好きの話を作成

お、HEAD が "8efecbd 猫好きの話と犬好きの話を作成" になったよ、と言われましたね。グラフを確認します。

```
$ git graph
* 5f26eb2  (hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
| * ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  (HEAD, master) 2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

うん、いいですね。master の位置がもとに戻っています。念のため、ファイルの内容ももどに戻っていることを確認してください。いいですね？

では、今度はここから「Fast-forward」にならないようにマージをしてみましょう。そのためには `git merge` に `--no-ff` というオプションを指定します。

```
$ git merge --no-ff hotfix
```

はい、これでエディタが立ち上がるマージになりました。コミットメッセージはデフォルトのままでいいのでテキストエディタを保存、終了しましょう。さて、ターミナルはどんな表示になっていますか？

```
Merge made by the 'recursive' strategy.
 cat_hater_said.txt | 2 +-
 cat_lover_said.txt | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)
```

うん、'recursive' stragy でマージされてますね。グラフも確認しましょう。

```
*   7090c03  (HEAD, master) 2013-05-06 Shinpei Maruyama Merge branch 'hotfix'
|\  
| * 5f26eb2  (hotfix) 2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
|/  
| * ff00bb6  (unify_styles) 2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

うん、マージコミットも作られています。良いですね。

hotfix ブランチはもう用無しなので消しちゃいましょう。

```
$ git branch -d hotfix
```

さて、これで master ブランチの内容をリリースしてしまいましょう。一安心ですね。

ところで、今回は Fast-fowarding せずにマージコミットを作る方式でマージを行いましたが、Fast-foward できるときは Fast-foward してしまうほうが好みであるというひともいます。このあたりは現在でも活発に宗教戦争が起こっているので、あなたの所属する組織やプロジェクトのやり方に合わせるといいでしょう。

## 文体統一のためのブランチにリテイクが発生

さて、2週間たち、ディレクターから文体統一のほうの作業にダメだしされました。「あの犬好きのひとが "ヘッヘッヘッヘッ" って言ってたくだり無くしちゃったの？ 俺あれ好きだったんだけど。あれ残してよ」だそうです。じゃあ unify_styles ブランチにもどって、そのための作業を行いましょう。

    $ git checkout unify_styles

で unify_styles ブランチをチェックアウトします。

そしたら、ファイルを編集しましょう。結果こうなりました。

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っているが、冗談じゃない。
人間が猫を飼ってやっているのである。そこを勘違いする猫は
本当にダメである。

その点犬は良い。犬は人間のことを「ご主人様」だとおもって
なついてくれる。それならばこそかわいがる気も起きるというものである。

犬は賢い。やはり犬がよい。犬はかわいい。

散歩に行ったあとに息が上がって "ヘッヘッヘッヘッ" と
なっている様など、ほんとうに良いものである。

猫が好きな人間は、頭がおかしいのではないだろうか。
```

変更をコミットしましょう。コミットメッセージは「"ヘッヘッヘッヘッ" という表現は残すようにした」でいいでしょう。

コミットしましたか？ ではグラフを確認。

```
$ git graph
* ddfdb5f  (HEAD, unify_styles) 2013-05-06 Shinpei Maruyama "ヘッヘッヘッヘッ" という表現は残すようにした
| *   7090c03  (master) 2013-05-06 Shinpei Maruyama Merge branch 'hotfix'
| |\  
| | * 5f26eb2  2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
| |/  
* | ff00bb6  2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

問題ないですね？

ではこのコミットの内容をディレクターに見てもらいましょう。見てもらったら、「OK」と言われました。ではこのブランチの編集内容をリリースするために、 master に反映させましょう！

## 文体統一のためのブランチをマージしよう

ブランチA の内容をブランチ B に反映させるには、ブランチ B を選択して、ブランチ A を merge ですね。今回は unify_styles を master に merge なので、「masterを選択して、unify_styles をマージ」です。

```
$ git checkout master
$ git merge unify_styles
Auto-merging cat_lover_said.txt
CONFLICT (content): Merge conflict in cat_lover_said.txt
Auto-merging cat_hater_said.txt
CONFLICT (content): Merge conflict in cat_hater_said.txt
Automatic merge failed; fix conflicts and then commit the result.
```

やってみたらなんか出た！怖い！！！！

怖くないです。英語の内容を見てみましょう。

「Auto-merging cat_lover_said.txt」は「cat_lover_said.txt を 自動マージしてるよ」でいいですね。そのあと、「CONFLICT (content): Merge conflict in cat_lover_said.txt」と言われています。オッ、これは「競合」ですね。よく考えてみると、hotfix でも cat_lover_said.txt の一番最後の行を編集していますし、unify_styles でも一番最後の行を編集してしまっています。このため、Git さんは「お、これどっちの状態を真とすればいいんだ？」と混乱してしまって、自動でマージが行えなかったわけですね。

その下の表示を見てみると、どうやら cat_hater_said.txt でも同様のことが起こっているようです。

そして最後に、「Automatic merge failed; fix conflicts and then commit the result.」と書かれていますね。「自動マージに失敗しちゃった＞＜ 競合を直して、その結果をコミットしてね！」と言われています。言われたとおりにしましょう。

と、そのまえに、なにはともあれ status を確認。

```
$ git status
# On branch master
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#
#	both modified:      cat_hater_said.txt
#	both modified:      cat_lover_said.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

見た事無い表示ですね。

「マージされてないパスがあるよ(コンフリクトを修正して git commit してね)。」だそうです。この「パス」ってのは「ファイルパス」のことですね。ファイルのことだと思ってくれてかまいません。

その下には「マージされてないパスは以下の通りだよ(修正したよってマーク付けるためには git add \<file\> ...してね)。 ・両方で変更されているファイル：cat_hater_said.txt ・両方で変更されているファイル cat_lover_said.txt」とありますね。

では、そのファイルの内容を見てみましょう。

* cat_hater_said.txt

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っているが、冗談じゃない。
人間が猫を飼ってやっているのである。そこを勘違いする猫は
本当にダメである。

その点犬は良い。犬は人間のことを「ご主人様」だとおもって
なついてくれる。それならばこそかわいがる気も起きるというものである。

犬は賢い。やはり犬がよい。犬はかわいい。

散歩に行ったあとに息が上がって "ヘッヘッヘッヘッ" と
なっている様など、ほんとうに良いものである。

猫が好きな人間は、頭がおかしいのではないだろうか。

<<<<<<< HEAD
猫好きな人間がいるのが不思議。
=======
>>>>>>> unify_styles
```

なにこれファイルが壊れてる怖い！

怖くないです。これは、「どこが自動でマージできなかったか」を Git さんが教えてくれたのです。

    <<<<<<< HEAD 

から

    =======

までは、「HEAD」で編集された内容です。今回は HEAD は master でしたね。master は hotfix の 内容を取り込んでいたので、hotfix のほうにあった内容「猫好きな人間がいるのが不思議。」がここに書かれています。

一方、

    =======

から

    >>>>>>> unify_styles

までには、unify_styles で編集された内容が書かれています。

ん？ 何も書かれていませんね。と思ったら、上のほうに「猫が好きな人間は、頭がおかしいのではないだろうか。」とありました。これが unify_styles に存在した内容ですね。どうやら Git さんはここまでは自動でマージしてくれたけど、「猫好きな人間がいるのが不思議。」という行はどうしたらいいのかわからなかったみたいですね。

では、この競合を、手動で解決しましょう。ふつうにファイルを編集してください。

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っているが、冗談じゃない。
人間が猫を飼ってやっているのである。そこを勘違いする猫は
本当にダメである。

その点犬は良い。犬は人間のことを「ご主人様」だとおもって
なついてくれる。それならばこそかわいがる気も起きるというものである。

犬は賢い。やはり犬がよい。犬はかわいい。

散歩に行ったあとに息が上がって "ヘッヘッヘッヘッ" と
なっている様など、ほんとうに良いものである。

猫が好きな人間が存在するというのが、私には不思議に思える。
```

うん、これでいいでしょう。競合を手動で解決したので、Git さんにこのファイルの競合は解決したよ、と教えて上げましょう。`git status` の表示によると、「use "git add \<file\>..." to mark resolution」でしたね。

    $ git add cat_hater_said.txt

はい、これで Git さんに「解決したよ」と教えて上げられました。`git status` で確認しましょう

```
$ git status
# On branch master
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#
# Changes to be committed:
#
#	modified:   cat_hater_said.txt
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#
#	both modified:      cat_lover_said.txt
#
```

良いですね。では残りの cat_lover_said.txt も同様に手動で編集します

```
猫は動物の一種である。

犬とならび、ペットとして飼われることの多い動物である。

猫は人間のことを下僕かなにかだと思っており、また人間も
人間で猫に使役されることを至上の喜びと思う傾向にある。

猫の魅力に取り付かれた人間は二度と猫の魅力から離れることが
できないとも言われている。

猫というのはかように本当にかわいいものである。

猫好きではない人間が存在するのが、私には不思議に思える。
```

これで競合が解決したので、`git add` しましょう。

```
$ git add cat_lover_said.txt 
```

`git status` で確認します。

```
$ git status
# On branch master
# All conflicts fixed but you are still merging.
#   (use "git commit" to conclude merge)
#
# Changes to be committed:
#
#	modified:   cat_hater_said.txt
#	modified:   cat_lover_said.txt
#
```

お、表示が変わりましたね。

「All conflicts fixed but you are still merging.(use "git commit" to conclude merge)」とありますね。日本語にすれば「競合は全部解決したけど、まだマージ中だよ。(マージを完成させるには、git commit してね)」です。

じゃあ言う通りにしましょう。

    $ git commit

はい、エディタが立ち上がりました。

```
Merge branch 'unify_styles'

Conflicts:
        cat_hater_said.txt
        cat_lover_said.txt
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#   (use "git commit" to conclude merge)
#
# Changes to be committed:
#
#       modified:   cat_hater_said.txt
#       modified:   cat_lover_said.txt
#
```

と書かれていますね。一応上から読みましょう。なになに？

```
unify_stylesをマージします。

競合していたファイル：
    cat_hater_said.txt
    cat_lover_said.txt
```

はい、そのとおりでしたね。次。

```
# マージコミットをしようとしているようですね。
# もしそうでないなら、以下のファイルを消してください。
#       .git/MERGE_HEAD
# で、そのあと、もう一度やってみてください。
```

マージコミットしてるので、これは無視でいいですね。

その下に書かれている内容はいつもと同じですので、割愛します。

うん、いいですね。じゃあこのままテキストエディタを保存して終了させましょう。

## マージの結果は？

はい、これでマージができました。結果を graph で確認しましょう。

```
$ git graph
*   16fa3b3  (HEAD, master) 2013-05-06 Shinpei Maruyama Merge branch 'unify_styles'
|\  
| * ddfdb5f  (unify_styles) 2013-05-06 Shinpei Maruyama "ヘッヘッヘッヘッ" という表現は残すようにした
* |   7090c03  2013-05-06 Shinpei Maruyama Merge branch 'hotfix'
|\ \  
| * | 5f26eb2  2013-05-06 Shinpei Maruyama 頭がおかしいという表現はまずいので修正
|/ /  
| * ff00bb6  2013-05-06 Shinpei Maruyama 文体を統一
|/  
* 8efecbd  2013-05-06 Shinpei Maruyama 猫好きの話と犬好きの話を作成
```

無事にマージコミットが作成されていますね！！いい感じです！

これで無事 unify_styles ブランチで行った変更も master ブランチに取り込まれたので、unify_stylesブランチは消してしまいましょう。

    $ git branch -d unify_styles
    
そして、すべての変更が反映されている master ブランチを、このままリリースしてしまいましょう。おつかれさまでした！！

今回のまとめです。

* なにか作業を行うときはブランチを切ってから行うと、いつでも「その作業を行う前の状態」に戻れるので便利
* 「分岐」してないブランチをマージするときには Fast-foward という手法が取られる
  * Fast-foward だと、マージコミットは作られず、たんにブランチが進められる
* Fast-foward したくないときには `--no-ff` というオプションを付ける
* マージしたときに競合が発生した場合には、手動でマージする
  * ファイルを手動で修正
  * `git add ＜修正したファイル＞`として「競合を直したよ」と Git に伝える
  * 全部直したら `git commit` でマージコミットを完成させる

良いでしょうか？

次回は、コミットの歴史を修正してしまう黒魔術「rebase」について見て行きましょう。

[ひとりでつかう - 過去を改変](08_rebase.md)