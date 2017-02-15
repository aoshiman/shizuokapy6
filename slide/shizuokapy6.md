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
 - 趣味でみかん作っています<!-- .element: class="fragment" data-fragment-index="5" -->
 - 今シーズンは10ｔ作りました(^^)v<!-- .element: class="fragment" data-fragment-index="6" -->

---

## 本日の発表について
- 昨年秋より、家庭の事情で AWS Lambda Python を触り始めて現在5プロジェクトを稼働させています<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- 5プロジェクトを3パターンに分け事例として共有したいと思います<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- AWS Lambda Pythonを開発する上で便利なツール、Lamveryについても解説します<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- このスライドはGitHub上でホスティングしています。connpassに同一スライドを載せましたのでご確認下さい<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## 皆さんはAWS Lambdaを知ってますかor使ってますか
<img src="slide/Vagrant.png" style="width: 30%; height: 30%"></img>

---

## AWS Lambdaとは
- サーバーをプロビジョニングしたり管理しなくてもコードを実行できるコンピューティングサービス<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- S3などのAWSリソースで発生したイベントからコードを実行する（Function-as-a-Service）
- 対応言語は Node.js、Java、C# および Python<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- 使用したコンピューティング時間に対してのみ課金<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## AWS Lambda Python
1. AWS Lambdaで使用できるPythonのバージョンは2.7<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
2. サードパーティモジュールを使う場合は実行ファイルと同じ場所に含める<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
3. boto3はもともと入っている<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## 事例その１
### 公開ブログのキャッシュパージ
---

### 前提
- ブログシステムは自作（Flask製）
- ローカルで記事を作成した後、全て静的ファイルに変換してAmazon S3にアップロードして公開
- S3の前段にCloudfrountというキャッシュシステムを立てている

---

## コード
- https://github.com/aoshiman/lambda_cloudfront_invalid

---


<img src="slide/shizuokapy6-1-1.png" style="width: 100%; height: 100%"></img>

---

<img src="slide/shizuokapy6-1-2.png" style="width: 100%; height: 100%"></img>

---

<img src="slide/shizuokapy6-1-3.png" style="width: 100%; height: 100%"></img>

---

## 事例その2
### 特定のメールを受信したら電話をコールする

---

### 前提
- 子供の習い事の送迎バスは到着10分前にメールをくれる
- しかしメールに気づかない場合が多い
- 音声などで知らせてくれるものはないか

---

## コード

- https://github.com/aoshiman/lambda_email2phone

---

<img src="slide/shizuokapy6-2.png" style="width: 100%; height: 100%"></img>

---

## 事例その3
### スケジュールイベントを使ったTwitterBotの運営

---

### 前提
- 2008年頃にほんの出来心で作成したTwitterBotをいまだに止められずに運用しているが、借りているVPSの費用も結構な負担に。。
- 2015年秋にcronライクなスケジュールイベントをAWS Lambdaがサポートしたのでずっとやってみたかった

---

## コード
- 某レシピサイトの話題のレシピ

- がんばれアドミン君
  + https://github.com/aoshiman/lambda_adminkun_bot

- 競馬ニュース
  + https://github.com/aoshiman/lambda_keibanews_bot

---

<img src="slide/shizuokapy6-3.png" style="width: 100%; height: 100%"></img>

---

## Lamveryについて
### Lamveryとは
- https://github.com/marcy-terui/lamvery
- Lambda functionのデプロイやFunctionそのものを含めた周辺機能の設定・管理を支援するツール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- Python製でサポート言語はPythonとNode.js<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- 同じようなツールにlambda-uploaderやApexがある

---

### Lamveryの良いところ
- 設定ファイルの形式がYAML（Jinja2をラップしているので変数も埋め込み可）
- virtualenv内で使用するとpipでインストールしたサードパーティモジュールをデプロイ時に自動アーカイブしてくれる
  + プロジェクトフォルダを汚さない
- KMS（AWS Key Management Service）のサポート
  + 特にencrypt-fileサポートによる恩恵
- 開発者が日本人（@marcy_terui）

---

## まとめ
- Vagrant便利だけどローカルディスク容量少ないとわりと積むことがある<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- vagrant-digitalocean使えばDigitalOceanをVagrantのboxとして使える<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- ローカルディスク容量節約できるしVirtualboxも不要になる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---
## 最後に
- Vagarantは仮想環境の構築、操作を容易にしてくれる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- VagarantとDigitalOceanの組み合わせはローカルリソースを意識せずに仮想環境を構築できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- VagarantとDigitalOceanとAnsibleを組み合わせることにより、自分好みの開発環境を構築できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---
