# Git

[よく使う Vim のコマンドまとめ](https://qiita.com/hide/items/5bfe5b322872c61a6896)

### バージョン管理の状態を確認する

`git status`

### マージする前の状態に戻す

`git merge --abort`

### リモートリポジトリから最新情報をローカルリポジトリに持ってくる

`git fetch`

### バージョン確認

`git --version`

### 登録されている名前とURLの確認

`git remote -v`

### origin の URL を変更

`git remote set-url origin 新しいURL`

### origin を削除

`git remote rm origin`

### ソースツリーの表示

`git log graff`

### 変更点を一旦退避させる

`git stash`

### Commit 削除

`git revert commitNo`

### Fork して push

```text
git remote add upstream [fork元 url]
git remote -v // Fork元が追加されているか確認
git pull upstream master
```

### 指定したコミットを、ブランチを変えて作り直したり、ひとまとめにしたりして、ログを綺麗にする

```text
git rebert [blunch]
git push -f
```

### GithubにPushしたコミットを削除する

* 作業中のブランチ名AをBに変更
* developをきれいにする　reset -HARD
* developからブランチAを作成
* ブランチAから、Bのコミットをチェリーピックする
* ブランチAをforce push
* ブランチBを削除

### きれいなブランチをつくる

```text
git checkout -b topic/## upstream/develop
git checkout -b topic/## origin/develop
```

## **特定のファイルのみ別ブランチからもってくる**

```text
git checkout [コミットID] [ファイルパス]
git checkout [ブランチ名] [ファイルパス]
```

### ローカルリポジトリを最新状態にする

```text
git fetch origin topic/master
git merge --no-ff origin/topic/master
git merge --no-ff 9809105
```

### ログを1行で出す

```text
git log --oneline
git log --oneline remotes/upstream/develop（ブランチ指定）
```

### リセット

```text
git reset --hard (commitNo/98aacec)
```

### git merge の取り消し

```text
git merge --abort
(コンフリクトの編集をしていないときに限りマージする前の状態に戻る)

git reset --hard HEAD
(編集した内容もマージもすべて取り消される)
```

### git pull の取り消し方法

```text
HEAD の移動履歴を表示
git reflog
```

### 参照、作業ツリー、インデクスを強制的に戻す

```text
git reset --hard HEAD@{1}

// ローカルの my-master ブランチを、origin 上の master ブランチに push する
git push origin my-master:master
```

`git diff` 

`git log` 

`git reset HEAD^` 

`git status` 

`git stash save -u` 

`git stash show` 

`git stash list`

`git stash drop`

