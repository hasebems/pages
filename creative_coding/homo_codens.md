# Homo Codens メモ

## 参加者
- 全員 35
- 女性：10？
- 大学の先生 7,8
- 大学生 5,6
- 美術商、美術館  2
- IT, デザイナー 5,6
- 山口市内 4,5

## １日目

AIとの関連性
高尾さんと相談
Generative Art

### 高尾
2008 OpenFrameworksとの出会い
母のキルト
JGAF
利己と利他を意図的に取り違える
Homo ludens
Homo faber

Generative
Rules
Randomness
Autonomy

Long Form Generative Art
作家がキュレーションせずにそのまま公開

人間がコードを書く
AIとともにコードを書く
はっかそん

ベルリン行動規範
書けない人でも参加できる

### 11:15-
プログラム書けない人歓迎といいながら、結構容赦ない
日常的に書いている人は簡単すぎるし、日常的に書いてない人に
とっては難しすぎる
fill は重い
色は “” で red とか書いて、色をクリックするとHSBで指定できる
生き生きとした絵 -> 座標変換

### 13:30 -
Cybernetics
  norbert wiener
  Max bence
  Claude Shannon  AI

CTG
Automatic Painting Machine(APM)

### 永松
Figma

自然、非人間を取り入れる
noFill();
beginShape();
vertex(x,y);
endShape();
リサジュー

Agent
LENIA
粘菌

### 絵の具（中山晃子）

Generative な例として Live Painting

### 麦
ジェネジェネ、チマチマ
道具　良さの山
RGB -> HSL
Creative Coding は道具
言語は思考を規定する
ツールは発想を規定する
Affordance of creative coding 
命令的　宣言的


## day2

adobe color
[https://color.adobe.com/ja/create/image]
色のパレットを作るツール

random()
引数に二次元配列を入れると、中の配列一つを選ぶ

ml5.js

### kubota
Creative Coding, Generative Art の違い
uncreative coding -> AI を使ったコーディング

自分自身を書き換えるプログラム
「わたしは不思議の輪」

colormind
[http://colormind.io/]

### 木原
生成AIを使った表現が抱えている課題

ゲームは「裁量権」そのものを表現する芸術
罪悪感、後悔を感じさせることができる

1. 意図を感じないものに関心が続かない
  - 過程が見えると意図を感じやすい
2. 潜在空間の回収であり、既視感の先に行けない
  - 生成というより回収的？
3. ありがたさを感じない
  - アウラ：一回性
  - 価値が下がっていく


### 田所
マルチモーダル
テキスト -> コード

### 100の絵を描く
1536*1536
1. image(png)
    1. movie(mp4 1sec)
2. Chord


### Chord Discussion感想
個々人の力量も、YCAMの舞台設定も凄すぎた
音楽に合わせて、スポットライトもぐるぐる動き、ミラーボールも回る
アート最優先で舞台を作っている
スクリーンは、演者の前に半透明のもの（コード用）、後ろに映像
一人目：コードはほとんどいじらない。コントローラ系で派手に音楽に
エフェクトをかける
二人目：冒頭、相当の長さでノイズ演奏、途中からリズムが入る
田所さん：もっとも聴きやすく美しい音楽。わかりやすすぎるかも。
四人目：コードをゼロから書き、恐らく事前仕込みなし。凄すぎ。Codingという意味で最も技巧派。映像の体操の女性のシルエットも美しかった。

全体的に映像の移り変わりが早い。同じ映像が5秒と続かないし、静止画像も無し。
四人目はそうでもなかったかもしれないけれど、みな映像が小刻み過ぎて、逆に様式のような
ものを感じてしまう。
確かに、普通の人にはそこまでの仕込みが出来ない。それがプロたる所以？

やるべきこと：
- もっと時間あたりの変化をつけること
- 全部書くことにこだわらない（分かってもらうことにそれほど意味はない）
- 画像エフェクトは、ある程度変える
- ただし、書いていることは強調したい
- エフェクトをかけたい。ピアノの場合、リバーブ？コーラス？
- 徐々に場面転換
- 即時変換対応


## day3
randomSeed()

095 gradient グラデーションの書き方
let gradient = drawingContext.createLinearGradient();

サイクロイド
