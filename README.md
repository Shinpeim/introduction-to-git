# Git をはじめからていねいに

by shinpei maruyama

## はじめに

本文書は Git の基本的な概念と使い方を、なるべくわかりやすく説明する意図で書かれています。本書を読む事で

* かんたんな Git の使い方ができる
* ブランチを使える
* 複数人での協調作業ができる
* わからないことがあったときに google で「どんな単語で調べればいいのか」 がわかる

ようになるのが目的です。

対象読者は、「Git を使ってみたいと思ってるけどなんか怖い」「業務で Git 使ってるけどよく詰む」「なんかいろいろ英語でてくるけどよくわかんないから読んでない」「なんとなく使ってるけどよくわかってない」というひとが対象読者です。逆に、「べつに Git で困ってない」「git flow おいしいです」「普通に毎日使ってる」「もうGitなしには生きられない」「彼氏が VCS 使ってなかった、別れたい」というようなひとは対象読者ではありません。

## 目次

1. [gitとはなんぞや](https://github.com/Shinpeim/introduction-to-git/blob/master/01_what_is_git.md)
1. [ひとりでつかう - はじめてのコミット](https://github.com/Shinpeim/introduction-to-git/blob/master/02_first_commit.md)
1. [ひとりでつかう - にどめのコミット](https://github.com/Shinpeim/introduction-to-git/blob/master/03_second_commit.md)
1. [ひとりでつかう - どんどんコミット](https://github.com/Shinpeim/introduction-to-git/blob/master/04_more_commits.md)
1. [ひとりでつかう - ブランチを知る](https://github.com/Shinpeim/introduction-to-git/blob/master/05_branch.md)
1. [ひとりでつかう - はじめてのマージ](https://github.com/Shinpeim/introduction-to-git/blob/master/06_merge.md)
1. [ひとりでつかう - もっとマージ](https://github.com/Shinpeim/introduction-to-git/blob/master/07_more_merges.md)
1. [ひとりでつかう - 過去を改変](https://github.com/Shinpeim/introduction-to-git/blob/master/08_rebase.md)
1. [みんなでつかう - ベアリポジトリとクローン,リモートリポジトリ](https://github.com/Shinpeim/introduction-to-git/blob/master/09_clone.md)
1. [みんなでつかう - push pull](https://github.com/Shinpeim/introduction-to-git/blob/master/10_push_pull.md)
1. 付録 逆引きコマンド集 - 執筆中

## ライセンス
<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.ja"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a>

この 作品 は <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.ja">クリエイティブ・コモンズ 表示 - 継承 3.0 非移植 ライセンスの下に提供されています。</a>

このドキュメントが訳にたった場合、著者に <a href="mailto:info@nekogata.com">焼き肉を寄付</a> したり、<a href="http://www.amazon.co.jp/registry/wishlist/155YU99M1BRGA">amazonから贈り物</a> をすることもできます。