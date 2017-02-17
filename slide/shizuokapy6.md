## 個人ユースにおける AWS Lambda Python 導入事例

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
 - 今シーズンは約8ｔ作りました(^^)v<!-- .element: class="fragment" data-fragment-index="6" -->

---

## 本日の発表について
+ 昨年秋より、家庭の事情で AWS Lambda Python を触り始めて現在5プロジェクトを稼働させています<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ 5プロジェクトを3パターンに分け事例として共有したいと思います<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ AWS Lambda Pythonを開発する上で便利なツール、Lamveryについても解説します<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
+ このスライドはGitHub上でホスティングしています。connpassに同一スライドを載せましたのでご確認下さい<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## 皆さんはAWS Lambdaを知ってますかor使ってますか
<img src="slide/Compute_AWSLambda.png"></img>

---

## AWS Lambdaとは
+ サーバーをプロビジョニングしたり管理しなくてもコードを実行できるコンピューティングサービス<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ S3などのAWSリソースで発生したイベントからコードを実行する（Function-as-a-Service）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ 対応言語は Node.js、Java、C# および Python<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
  - Node.js – v0.10.36, v4.3.2 (推奨)
  - Java – Java 8
  - Python – Python 2.7
  - .NET Core – .NET Core 1.0.1 (C#)
+ 使用したコンピューティング時間に対してのみ課金<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---

## 無料枠について
+ 個人ユースで使用するならばより強く無料枠を意識せねばならない<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
  - １ヶ月あたり100万リクエストが無料、および400,000 GB/秒の実行時間が無料


| メモリ（MB） | 無料利用枠の秒数／月 | 時間換算 |
| ------ | ----------- | ----------- |
| 128 | 3,200,000 | 889h |
| 256 | 1,600,000 | 444h |
| 1024 | 400,000 | 111h |

---

## Python で はじめる AWS Lambda
### コンソール内で作成する場合

---

### コンソール外で作成する場合
### サードパーティモジュールを含めたい場合
```
cd /path/to/project-dir

pip install module-name -t .
```
#### zipで固める
+ 注）フォルダが入れ子にならないように
```
cd /path/to/project-dir

zip -r upload.zip *
```
#### アップロードする

---

## 事例その１
### 公開ブログのキャッシュパージ
---

### 前提
+ ブログシステムは自作（Flask製）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ ローカルで記事を作成した後、全て静的ファイルに変換してAmazon S3にアップロードして公開<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ S3の前段にCloudfrountというキャッシュシステムを立てている<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

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
+ 子供の習い事の送迎バスは到着10分前にメールをくれる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ しかしメールに気づかない場合が多い<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ 音声などで知らせてくれる仕組みが必要<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## コード

+ https://github.com/aoshiman/lambda_email2phone

---

<img src="slide/shizuokapy6-2.png" style="width: 100%; height: 100%"></img>

---

## 事例その3
### スケジュールイベントを使ったTwitterBotの運営

---

### 前提
+ 2008年頃にほんの出来心で作成したTwitterBot達をいまだに止められずに運用しているが、借りているVPSの費用も結構な負担に。。<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ 2015年秋にcronライクなスケジュールイベントをAWS Lambdaがサポートしたのでずっとやってみたかった<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
  - 作成したTwitterBotはcronで稼働しており、AWS Lambdaのようなステートレスなサービスとは性質が似ているので手直しが少ないと思った
  
---

## コード
+ 某レシピサイトの話題のレシピ （ https://twitter.com/cookpad_recipe ）
  - https://github.com/aoshiman/lambda_cookpad_recipe
  
+ がんばれアドミン君（ https://twitter.com/ad_min_kun ）
  - https://github.com/aoshiman/lambda_adminkun_bot

+ 競馬ニュース( https://twitter.com/keiba_news )
  - https://github.com/aoshiman/lambda_keibanews_bot

---

<img src="slide/shizuokapy6-3.png" style="width: 100%; height: 100%"></img>

---

## Lamveryについて
### Lamveryとは
+ https://github.com/marcy-terui/lamvery
+ Lambda functionのデプロイやFunctionそのものを含めた周辺機能の設定・管理を支援するツール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ Python製でサポート言語はPythonとNode.js<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ 同じようなツールにlambda-uploaderやApexがある<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->

---

## Lamveryの良いところ
+ 設定ファイルの形式がYAML（Jinja2をラップしているので変数も埋め込み可）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ virtualenv内で使用するとpipでインストールしたサードパーティモジュールをデプロイ時に自動アーカイブしてくれる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
  - プロジェクトフォルダを汚さない
+ KMS（AWS Key Management Service）のサポート<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
  - 特にencrypt-fileサポートによる恩恵は大きい
  - AWS Lambda には暗号化可能な環境変数をサポートしているが、既存設定ファイル等を丸ごと暗号化できた方が楽
+ 開発者が日本人（@marcy_terui）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->
  - 安心感あります
  
---

## Lamveryのzsh補完関数作りました
+ サブコマンド12種類、オプション22種類あってとても覚えられないので。。
+ https://gist.github.com/aoshiman/d2e93f986802f5a359b4dc6f25eff1a3
+ 使い方 https://blog.aoshiman.org/entry/149/

---

## まとめ
+ AWS Lambda は用途がハマれば大変便利<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
+ サーバーレスは気持ちが楽<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
+ 無料枠情報は常に把握しておきたい<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
+ AWS Lambda で Python を選択するなら Lamvery を使おう<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

---
