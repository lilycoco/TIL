# Docker

## S3

### minio

#### Bucket

{% embed url="https://qiita.com/terukizm/items/6850aa316de425e3e3b4" %}

S3上のルートディレクトリみたいなもの。  
S3はサービスであって、立ち上げただけだと中身が虚無なんですね。。  
なので、S3立ち上がったらpenguinっていうディレクトリを用意してね、っていう処理です！  
で、僕らはそのpenguinっていうディレクトリにファイルを置いていけるわけですね

ややこしいから読み飛ばしていいけど、  
正確にはS3にディレクトリっていう概念はなくて、バケットというコンテナがあって、その中に無数のオブジェクトがあるんだ。  
まあ要はS3全体が連想配列的なkey-valueのオブジェクトみたいな状態ってことだね。  
ちなみにバケットは全リージョンで一意な名前しか付けられないんですよ。

{% embed url="https://docs.aws.amazon.com/ja\_jp/AmazonS3/latest/dev/Introduction.html\#BasicsBucket" %}



