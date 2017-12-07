# iPerf 導入手順
## 1. EPEL リポジトリの追加
### 1.1. EPEL RPM の取得
```sh
$ sudo wget https://ftp.yz.yamagata-u.ac.jp/pub/linux/fedora-projects/epel/6/x86_64/epel-release-6-8.noarch.rpm
```
### 1.2. EPEL リポジトリリストの設定
```sh
$ sudo rpm -ivh epel-release-6-8.noarch.rpm
```
### 1.3. EPEL リポジトリリストの確認
```sh
$ sudo yum repolist
```
## 2. iPerf3/iPerf インストール 
```sh
$ sudo yum -y --enablerepo=epel install iperf iperf3
```

## 3. iPerf3 起動
### 3.1. サーバ側起動
```sh
$ iperf3 -s
```

### 3.2. クライアント側起動
- iperf3 ラッパーシェルを使用（iperfw.sh）

```
$ ./iperfw.sh  -c 146.56.0.205 -t 5 -P 3
```

- c: サーバ側アドレス

- t: 計測時間

- P: 同時接続数

#### 備考
標準では 8KB. 16KB, 32KB の送出となっている
シェル内の以下の部分を書き換えて、初期サイズと倍化繰り返し数を設定
```
INITIAL_LENGTH=8
RECURRENCE=3
```