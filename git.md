# Git

[よく使う Vim のコマンドまとめ](https://qiita.com/hide/items/5bfe5b322872c61a6896)

## **きれいなブランチをつくる**

    git checkout -b topic/## upstream/develop
    git checkout -b topic/## origin/develop

## **特定のファイルのみ別ブランチからもってくる**

    git checkout [コミットID] [ファイルパス]
    git checkout [ブランチ名] [ファイルパス]

### ローカルリポジトリを最新状態にする

    git fetch origin topic/master
    git merge --no-ff origin/topic/master
    git merge --no-ff 9809105

### ログを1行で出す

    git log --oneline
    git log --oneline remotes/upstream/develop（ブランチ指定）

### リセット

    git reset --hard (commitNo/98aacec)

### git merge の取り消し

    git merge --abort
    (コンフリクトの編集をしていないときに限りマージする前の状態に戻る)
    
    git reset --hard HEAD
    (編集した内容もマージもすべて取り消される)

### git pull の取り消し方法

    HEAD の移動履歴を表示
    git reflog

### 参照、作業ツリー、インデクスを強制的に戻す

    git reset --hard HEAD@{1}

    // ローカルの my-master ブランチを、origin 上の master ブランチに push する
    git push origin my-master:master
