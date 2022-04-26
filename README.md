# GitHubの公開鍵を利用した簡単な暗号化のシェルスクリプト試作版

GitHubユーザーの公開鍵はすべて文字通り公開されており、ユーザー名が分かれば参照できます。

そこでGitHubユーザー名に宛てて平文を簡単に暗号文に変換し、受け取ったユーザーもそれを簡単に復号するシェルスクリプトを試作しました。

このスクリプトはMacOSを前提にしていますが、MacOSに依存する`pbcopy`と`pbpaste`コマンドを変更することで他のOSでも利用できます。

# openssl@3のインストール

MacOS標準の`openssl`では、RSA-OAEP SHA256に対応していないので、Homebrewで`openssl@3`をインストールしてください。

    brew install openssl@3

インストールした`openssl`コマンドを使うため、パスを通す必要があります。

    echo 'export PATH="/opt/homebrew/opt/openssl@3/bin:$PATH"' >> ~/.zshrc

## githubenc

平文をクリップボードにコピーし、次のコマンドを実行します。パラメータはGitHubユーザー名です。自分のユーザー名などに変更してください。

    ./githubenc miyanaga

暗号文がクリップボードにコピーされます。

    Hs3qBDUK5KPMMP3rA/4zL2XQqKcCjv5qQ0yC/cX1oWPcZw0v5qH6NgK7SyY7jvlq
    qzP2V6S190emAMkbr4LFNXWnuqUkOgfKBsgXgbzmYldVcL5HX+cr2B2V+VqrosrD
    /lx1msLuygnQ5O/MRDOV7sbQ6KXOcSGKTCGfU7yvfMn4mOb3D1SCNZDrrh4BS67T
    oxZvEMTS3EFVm6OPpk7SvzyPx2xGFUTBBXKv/vmJpAKIEPa1Ihx2uXA4uUgdQ01y
    RpLsNN/pckXXKO9xrvS5a7bAwWkcCH/UL0wppNEiPwz+xv466PHWYfVLHc0LoMZ1
    ndsiDjSKxSgLNRUjsiQ7Yzah1d2tOghlimp2LPbrsmhfleNxMng73iNMu3i0Fxrl
    eHS0GL6SvJti1FrKfLuHkxQ+AY+cqE9Ay8hYIF/htdY6QolaZffTjC2WHpgE6gzf
    bV9fQzV9sc7n6Sjx0fABQ1CZiS388S4o/N2lDCZDfLOP2vnMLRw2DqPRmYiDK5Ai
    7R+5Mq7TdpSgG4pUkNhPFr6pg+vI0gbYDExebK5ZZwn5mRwiy4gArly7dwCfj2Jt
    tapimmULKAde0pY2tc77M8RIbH2cITRnzMyCi1iIFoW+jBHZxBHrz8Ouo+L+Ct0G
    qfgTorQIbVai3zGHoM3QZqUL8rVeAph6RMxOsjr1wsU=

## mydec

利用する前に、PEM形式の秘密鍵を`~/.ssh/id_rsa.pem`に配置してください。

最近の`ssh-keygen`はデフォルトでPEMではなくOpenSSH独自の秘密鍵を出力するようです。

`~/.ssh/id_rsa`の1行目が`-----BEGIN OPENSSH PRIVATE KEY-----`の場合、それに該当します。

例えば以下のようなコマンドでPEMに変換します。

    cp -a ~/.ssh/id_rsa ~/.ssh/id_rsa.pem
    ssh-keygen -p -m pem -f ~/.ssh/id_rsa.pem

次にクリップボードに暗号文が入った状態で、

    ./mydec

を実行すると、クリップボードに平文がコピーされます。