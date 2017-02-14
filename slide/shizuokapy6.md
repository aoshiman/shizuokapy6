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
- このスライドはGitHub上でホスティングしています。connpassに同じものを載せましたのでご確認下さい<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

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
1. 2.7<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
2. サードパーティモジュール<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
3. boto3はもともと入っている<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
---

## 事例その１
### キャッシュクリア
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

<img src="slide/shizuokapy6-2.png" style="width: 100%; height: 100%"></img>
---

## 事例その3
### スケジュールイベントを使ったTwitterBotの運営

---

<img src="slide/shizuokapy6-3.png" style="width: 100%; height: 100%"></img>

---

## Lamveryについて
- https://cloud.digitalocean.com/
- 1時間1円で使えるクラウドホスティングサービス（というふれこみ）<!-- .element: class="fragment highlight-current-blue" data-fragment-index="1" -->
- 実際には従量制の512MBプランで$0.007/hr<!-- .element: class="fragment highlight-current-blue" data-fragment-index="2" -->
- 殆どの機能をAPIから利用できる<!-- .element: class="fragment highlight-current-blue" data-fragment-index="3" -->
- なによりもVagrantのプロバイダ（VirtualBoxの替わり）として機能する<!-- .element: class="fragment highlight-current-blue" data-fragment-index="4" -->

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
