## 個人ユースにおける AWS Lambda python 導入事例

### 2017.2.18 Shizuoka.py#6 & shizudev
### @aoshiman
---

## 自己紹介
+ @aoshiman
 - 食品会社勤務の社内SE<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
 - 仕事ではWindows Serverのお守りとVBA書いてます<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
 - Pythonは2008年くらいから触っています（もっぱら趣味）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
 - FlaskというPython製Webアプリケーションフレームワークが好きです<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->
 - しがないみかん農家の長男です<!-- .element: class="fragment" data-fragment-index="5" -->

---

## 本日の発表について
- 昨年秋より、家庭の事情で AWS Lambda Python を触り始めて現在5プロジェクトを稼働させています<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- 5プロジェクトを3パターンに分け事例として共有したいと思います<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- AWS Lambda Pythonを開発する上で便利なツール、Lamveryについても解説します<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- このスライドはWeb上でホスティングしています。connpassに同じものを載せましたのでご確認下さい<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## 皆さんはAWS Lambdaを知ってますかor使ってますか
<img src="slide/Vagrant.png" style="width: 30%; height: 30%"></img>

---

## AWS Lambdaとは
- 仮想環境の設定と操作を容易にするコマンドラインツール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- Rubyで書かれている<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- Windows OSX Linux対応<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- プロビジョニングツールと組み合わせることで開発環境がすぐ手に入る<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## AWS Lambda Python
```
$ vagrant init ubuntu/trusty64
```
1. Vagarantはboxというイメージファイルが必要で、使用するイメージがローカルに無ければaddコマンドでダウンロードします<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
2. Vagarantでは仮想環境の設定をVagrantfileという名前の設定ファイルに記載します<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
3. 上記コマンドはubuntu/trusty64（14.04）用設定ファイルをinitで作成しますが、該当イメージがローカルにない場合addコマンド無しでAtlasというVagrantboxの公式リポジトリからUbuntu用boxをダウンロードします<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
---

## 最近起こった困ったこと

- 撮りためていた子供のビデオデータをMacBook Air にうっかり流しこんでしまった <!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- その結果<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- ディスク容量90%超に<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- しかたがないのでせっかくダウンロードしたboxファイルを削除する羽目に。。<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

```
$ df -ah
Filesystem      Size   Used  Avail Capacity  iused   ifree %iused  Mounted on
/dev/disk1     112Gi  101Gi   11Gi    91% 26497364 2824362   90%   /
devfs          184Ki  184Ki    0Bi   100%      638       0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0       0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0       0  100%   /home
```

---

### ディスク容量がひっ迫してるので気軽にVagrantを試せなくなってしまった
## つらい(ノД`)・゜・。<!-- .element: class="fragment" data-fragment-index="1" -->

---

### だが救世主現る
<img src="slide/DO_Logo_Vertical_Blue.png" style="width: 50%; height: 50%"></img>

---

## DigitalOceanとは
- https://cloud.digitalocean.com/
- 1時間1円で使えるクラウドホスティングサービス（というふれこみ）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- 実際には従量制の512MBプランで$0.007/hr<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- 殆どの機能をAPIから利用できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- なによりもVagrantのプロバイダ（VirtualBoxの替わり）として機能する<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## vagrant-digitalocean
- https://github.com/devopsgroup-io/vagrant-digitalocean
- DigitalOceanをVagrantのプロバイダとするプラグイン
- これがあればVirtualBoxはもう必要ない（かもしれない）
- インストール方法
```
$ vagrant plugin install vagrant-digitalocean
```

---

## 通常のVirtualBoxをプロバイダとしたVagrantfile
```
   # -*- mode: ruby -*-
   # vi: set ft=ruby :
   Vagrant.configure(2) do |config|
     config.vm.box = "ubuntu/trusty64"
     config.vm.network "forwarded_port", guest: 80, host: 8080
   end
```

---

## DigitalOcean用のVagrantfile
```
Vagrant.configure(2) do |config|
  config.vm.hostname = ENV["VM_HOSTNAME"]
  config.ssh.forward_agent = true

  config.vm.provider :digital_ocean do |provider, override|
    override.vm.box = "digital_ocean"
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
    override.ssh.private_key_path = "~/.ssh/digitalocean/id_rsa.digitalocean.com"
    override.ssh.username = ENV["SSH_USERNAME"]
    provider.token = ENV["DIGITALOCEAN_API_KEY"]
    provider.image = "ubuntu-14-04-x64"
    provider.region = "sgp1"
    provider.size = "512mb"
    provider.setup = true
    provider.ssh_key_name = ENV["DIGITALOCEAN_SSH_KEY_NAME"]
  end
end
```
- ENV["SSH_USERNAME"]はRubyで環境変数を取得する書き方。因みにPythonでの書き方は↓
- import os;os.environ.get("SSH_USERNAME")<!-- .element: class="fragment" data-fragment-index="1" -->
---

## DigitalOcean用Vagrantfileの解説
### overrideとprovider項目について
---

## override項目
- override.vm.box_url
  - DigitalOcean用のダミーURLを指定
```
override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
```

- override.ssh.private_key_path
  - DigitalOceanとSSH通信する為の秘密鍵を指定
```
override.ssh.private_key_path = "~/.ssh/digitalocean/id_rsa.digitalocean.com"
```
  - vagrant up時に公開鍵が未登録の場合は自動登録される<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## provider項目その1

- トークン
```
provider.token = ENV["DIGITALOCEAN_API_KEY"]
```
  - APIを操作するAccess Token（管理画面より取得）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->

- イメージ
```
provider.image = "ubuntu-14-04-x64"
```
  - DigitalOceanで用意されているimageを選択（現在62種類）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->

---

## provider項目その2

- リージョン
```
provider.region = "sgp1"
```
  - DigitalOceanにおけるリージョン（現在11箇所）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

- セットアップユーザー
```
provider.setup = true
```
  - Trueにすると指定したユーザーがboxイメージに存在しなければ、sudoユーザーとして作成<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->
  - DigitalOceanの場合、ユーザー指定しなければrootのみ有効となる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="5" -->

---

## Vagrantの実行

```
$ vagrant up --provider=digitalocean
```

- シャットダウン(vagrant halt)してもドロップレットがある限り課金されるので必ず削除<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->

```
$ vagrant destroy
```
- ドロップレットについて
  - DigitalOceanの場合、AWSで言うところのインスタンスのことをドロップレットと呼びます<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- 課金について
  - 約$0.01/hrの課金ですが、クレカでなくPaypalで支払うとチャージ方式になるのでうっかりしてても残高無くなった時点で止まる（はず）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## デモ
- DigitalOceanのウェブサイトにドロップレットが一つもないことを確認
- 公開鍵が登録されていれば削除
- vagrant up
- ドロップレットが作成されていることを確認
- 公開鍵が作成されていることを確認
- vagrant destory

---

## ここまでのまとめ
- Vagrant便利だけどローカルディスク容量少ないとわりと積むことがある<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- vagrant-digitalocean使えばDigitalOceanをVagrantのboxとして使える<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- ローカルディスク容量節約できるしVirtualboxも不要になる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## 良いことばかりではない
- 一番近いリージョンがシンガポール
  - ping...
    ```
    --- 139.59.xxx.xx ping statistics ---
    10 packets transmitted, 10 received, 0% packet loss, time 9011ms
    rtt min/avg/max/mdev = 68.071/68.706/72.027/1.125 ms

    --- 139.59.xxx.xx ping statistics ---
    29 packets transmitted, 29 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 115.949/139.437/163.994/11.019 ms
    ```
  - 東京リージョン欲しい。。<!-- .element: class="fragment" data-fragment-index="1" -->

---

## VagrantとDigitalOceanとAnsibleでPython開発環境を構築する
## やっとPythonの話に<!-- .element: class="fragment" -->

---

## Ansibleとは
- https://www.ansible.com/
- Pythonで書かれたエージェントレスな構成管理ツール
- Ansible社が中心となって開発
  - Ansible社は昨年Red Hat社に買収されました（買収額150億超）
  - Ubuntu大好きっ子としては不安なところもある

--

## 余談
- 因みにAnsibleの開発者はMichael DeHaan氏

--

- さかのぼること約3年前、まだAnsibleがここまでメジャーになっていなかった頃
<img src="slide/tweet1.png" style="width: 100%; height: 100%"></img>

--

- エゴサしたDeHaan氏が返答！
<img src="slide/tweet2.png" style="width: 80%; height: 80%"></img>
- 当時は開発者ご本人がエゴサしてリプライしてくれる時代でした
--

- 尚、Michael DeHaan氏は昨年Ansible社を辞めています
- http://michaeldehaan.net/post/109595670406/happy-trails-ansible

---

## Ansibleの実行環境について
- 管理ホスト側にはPython2.6以上が必要（Python3未対応）
- 対象ホスト側にはPython2.4以上が必要（Python3未対応）
- SSHが繋がること（OpenSSHを使用、一部でparamikoを使用）

---

## Ansibleのインストール
- OSXの場合（注）何故か公式ドキュメントにはbrewでのインストールが載っていない
```
$ brew install ansible
```

- Ubuntuの場合PPAの追加が必要になります
```
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

- Via pip もしかしたらOSXもこちらのほうがよいかもしれない
```
$ sudo pip install ansible
```

---

## Ansibleで必要な3つの構成ファイル
- inventoryファイル（ini形式）
  - 実行対象となるホストのIPやserver名をを記載します。また複数ホストをグルーピングしたりします<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- ansible.cfgファイル
  - Ansible自体の設定を記述した設定ファイル（ini形式）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- playbookファイル
  - taskと呼ばれるひとつひとつの動作を記述したファイル（YAML形式）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## Ansibleの実行
- 基本的にはplaybookにhosts、taskを記述し実行

~~~yaml
- hosts: all
  tasks:
  - name: Install python3-dev python3-pip
    apt: name={{ item }} state=latest update_cache=yes
    with_items:
    - build-essential
    - python3-dev
    - python3-pip
  become: yes
~~~

~~~
$ ansible-playbook test.yaml --ask-sudo-pass
~~~

---

## VagrantとAnsibleを連携させる
- Vagrantは主要な構成管理ツールと標準で連携できる
  - Vagrantfileにこのように記述します
~~~ruby
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "provision.yml"
    ansible.verbose = "VV"
  end
~~~


---

## AnsibleでPython開発環境を構築する
---

### Pythonの開発環境を構築するplaybookのなかでポイントとなる箇所を次に挙げていきます
---

### 1. python-dev pipのインストール

~~~yaml
- name: Install python3-dev python3-pip
  apt: name={{ item }} state=latest update_cache=yes
  with_items:
  - build-essential
  - python-dev
  - python3-dev
  - python-pip
  - python3-pip

- name: pip install
  pip: name={{ item }} executable=pip2
  with_items:
  - virtualenvwrapper
~~~
- apt: apt-get instllを実行するモジュール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
  - update_cache: apt-get update をキャシュするオプション（初回だけupdateさせる）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- with_items: 列挙したものを{{ item }}に繰り返す<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- pip: pipを実行するモジュール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->
  - executable: pipを実行するPythonのversionを指定<!-- .element: class="fragment highlight-current-blue" data-fragment-index="5" -->

---

## 2. gitプロジェクトのclone

~~~yaml
- name: Git clone from bitbucket
  git: repo=git@bitbucket.org:aoshiman/flasklog.git
       dest=~/project/flasklog
       version=master
       accept_hostkey=yes
  become: no
~~~

- git: git cloneさせるモジュール
  - repo: リポジトリ
  - version: ブランチの指定
  - become: sudoするかどうか
    - Ansible1.9よりsudo から becomeへ変更されました
  - accept_hostkey: 次ページで説明

---

### ssh-agentを使ったGitプロジェクトのclone
#### ssh-agentを使って管理ホストの秘密鍵を対象ホストに転送させてgit cloneさせます
- まずssh-agentがForwardAgentできるようにします
  - ansible.cfg
  ```
  [defaults]
  private_key_file = ~/.ssh/bitbucket.org/id_rsa.bitbucket.org
  [ssh_connection]
  ssh_args = -o ForwardAgent=yes
  ```

  - Vagrantfile
  ```
  config.ssh.forward_agent = true
  ```

---

- 次にssh-agentの起動と秘密鍵の登録をします
  - Linuxの場合
  ```
  $ eval `ssh-agent`
  $ ssh-add ~/.ssh/bitbucket.org/id_rsa.bitbucket.org
  $ ssh-add -l
  2048 xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx
  /home/aoshiman/.ssh/bitbucket.org/id_rsa.bitbucket.org (RSA)
  ```

  - OSXの場合
  ```
  $ ssh-add -K ~/.ssh/bitbucket.org/id_rsa.bitbucket.org
  $ ssh-add -l
  2048 x:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx
  /Users/aoshiman/.ssh/bitbucket.org/id_rsa.bitbucket.org (RSA)
  ```
    - OSXの場合、ターミナルにログインするとagentは起動する（はず）

---

- 最後にOpenSSHの警告メッセージを消します

  - 方法1 .ssh/configに記述する
```
StrictHostKeyChecking no
UserKnownHostsFile=/dev/null
```

  - 方法2 ansible.cfg に記述
```
[defaults]
host_key_checking = False
[ssh_connection]
ssh_args = -o UserKnownHostsFile=/dev/null
```

  - 方法3 ansible.cfg と playbookに記述
```
# ansible.cfg
[ssh_connection]
ssh_args = -o UserKnownHostsFile=/dev/null
#
# playbook
- name: Git clone from bitbucket
accept_hostkey=yes
```

--

~~~~
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx.
Please contact your system administrator.
Add correct host key in /root/.ssh/known_hosts to get rid of this message.
Offending RSA key in /root/.ssh/known_hosts:11
RSA host key for xxx.xxx.xxx.xx has changed and you have requested strict checking.
Host key verification failed.
~~~~

---

### 3. バックグランドプロセスの持続

~~~yaml
- name: Start Flasklog
  shell: >
        /bin/bash -lc "source ~/.virtualenvs/flasklog/bin/activate
        && cd ~/project/flasklog
        && ~/.virtualenvs/flasklog/bin/python3 ./manage.py runserver &"
  async: 10
  poll: 0
~~~

- AnsibleではSSHコネクションが切れると & などで起動したバックグランドプロセスはkillされるので下記のオプションが必要

  - async:0より大きい整数を設定（0は非同期の無効）
  - poll:プロセスを持続させるには0にする
    - 非同期で実行しているタスクが終了しているかどうかチェックする間隔を指定
    - 0にすることでfire & forget（点けっぱなし）になる

---

## デモ
- この日用にサンプルデモを用意したかったのですが、時間が無くいつもやっていることをデモにさせて頂きます
- VagrantとAnsibleを使い、DigitalOceanのシンガポールリージョンにUbuntu14.04を立ち上げ、Flask製blogを起動させます
- 確認作業としてFlaskが起動しているUbuntuにhttpアクセスしてみます
- http://（DigitalOceanがアサインしたip）:5000にアクセス

---

## まとめ
- Vagarantは仮想環境の構築、操作を容易にしてくれる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- VagarantとDigitalOceanの組み合わせはローカルリソースを意識せずに仮想環境を構築できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- VagarantとDigitalOceanとAnsibleを組み合わせることにより、自分好みの開発環境を構築できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## 実行環境
- OS X Yosemite10.10.5
- Python2.7.11
- Vagrant1.8.4
- vagrant-digitalocean0.9.0
- ansible2.1.0.0
