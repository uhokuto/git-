# 同じPCで複数のgit, git_hubを使い分ける方法
githubアカウントを授業用とプライベートの2種類開設して、1台のPCからディレクトリ別にどちらかのgithubにgit pushを振り分ける環境設定をしようとしたら、めちゃくちゃハマりました。:  
　２つのgithubそれぞれ用の秘密鍵公開鍵ペアを生成して登録するだけではダメなのです。秘密鍵を格納しているディレクトリ.ssh内に、git pushの振り分けをするための専用のconfigファイルをgitの一般的configファイル(gitconfig)とは別に作る必要があります。この環境設定方法は以下のサイトが詳しかったです。  
https://zenn.dev/hironorioka28/articles/35a287ffbcf533  
ただし、configは拡張子がない妙なファイルで作成する必要があり、git touchという変なコマンドで作成する必要があります。これに気が付くのに数時間！  
https://qiita.com/wannoko/items/3fdc536e42b6e32afe8c  
これで完璧かとおもったら、403エラー。何度見直しても正しい！このエラー事象をググったら git remote set-url origin という意味不明なおまじないで解決するという記事があり試したらようやく解決しました。  
https://qiita.com/tatsunoshin/items/3d8b15fe50e85c918761  
ちなみに秘密鍵は、~ (ユーザーディレクトリ）直下の.ssh内に作成するというようにあちこちで書かれてますが、.sshディレクトリはどこにおいてもgitが自動的にパスを探すらしいです。(もしかすると、mainのrsaキーだけは、uhoku直下の.ssh内に入れとく必要がある)

なお、1人で複数のgithubアカウントを使い分けるのは、規約上は認められていないらしい。そのため、どちらかのgithubアカウントが何かのきっかけで急にアクセスできなくなることがあるらしい。その場合は、アクセスできなくなった方のすべてのリポジトリを一度 git remote rm して、再度以下のリンクのとおり、git remote addするときにリポジトリをhttps: でなくて、git:（つまりssh)で再登録するとアクセスが復活する

https://qiita.com/kaitoland/items/01be88f08236703f7ca6

複数の端末から同じリポジトリをpushしたために、一度pullしてpushしなおさなければならないケースでは、面倒なので、同期がとれてなくても強制的にpushする方法がある

https://qiita.com/Takao_/items/5e563d5ea61d2829e497

