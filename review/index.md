
# 【About】

元々レビューする時に気をつけてたポイントの話だったが、肥大化してレビューで指摘したこと・されたこと集になってきたのでそのようなメモにする。
他の人のレビュー指摘を見て、自分も同じ轍を踏まないために陥りがちなポイントをアウトプットし言語化したもの。

【目次】

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3325886479 (※初学者向け)

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3325984787 (※1～2年目向け)

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3326967874 (※頻出ロジック系)

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3325952002 

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3325984769 

https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3372548226 

(初学者がこれ見るとビビるかもなのでアドバイス)
小難しいこと書き並べてますけど、大事なのはとにかく手を抜くことだと思ってます。脳の使用メモリを最小限にして、なんか書いててダルいからもっとラクな方法ないの？ってChatGPTとかに聞けばこういうのは自然に身につく気がします。仕事なんかテキトーにやって終われるのが一番だと思ってます。僕はコード書くの嫌いなので。



## (参考1) 僕が過去に読んだ本
| タイトル | 感想 |
| ---- | ---- |
| [リーダブルコード ―より良いコードを書くためのシンプルで実践的なテクニック (Theory in practice) ](https://amzn.to/3EbhhTT) | 有名なベストセラー。初学者はまず一読してみると良さそう。レビュー指摘されてその部分を読み直すみたいな使い方だと定着しそう。<br>当たり前のことが書いてある感あるが、**当たり前のことが当たり前にできる、という人がこの業界結構少ない**ので、特に1～2年目の人とかこれ読んで「別に普通じゃね？」って思えるかどうか試すと良さそう。個人的にそれだけで単価80万レベルはいける。<br>中級者以上でも感覚でやってるのと知識があった上でやってるのとでは差異があるので、ある程度の人でも読んでみると**確固たる自信につながる。** |
| [SQLアンチパターン](https://amzn.to/3PVjO7g)  | これも有名な本っぽい。初学者にはおすすめ。<br>隣接リストとか定義しちゃいがちなので知識としては大事。買ってよかった。<br>僕が最も参考になった例→https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3255762959 | 
| [Go言語 100Tips ありがちなミスを把握し、実装を最適化する impress top gearシリーズ ](https://amzn.to/4hyFMIY) | Goは現状独学なので、なんか本でも読もうかなと思って買ったもの。Goという言語の特性やTipsを理解するのに良かった。[関数オプションパターンとか目から鱗だった](https://io-sys.atlassian.net/wiki/spaces/iosyskeiri/pages/3269820421/Go+100Tips+wip#%E9%96%A2%E6%95%B0%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3) |
| [Joel on Software ](https://amzn.to/40OUW7h) | 客先のフリーランスとド平日の深夜1時半まで飲んでた時におすすめされた本。(笑) Excelの開発者の本かな？技術というより価値観の本。興味深い内容がいくつかある。 |
| [ザ・ゴール ](https://amzn.to/3PTLARt) | 勧められたので読んだ本。どっちかというとマネジメント寄りの本だが、小説みたいに読める。プロジェクトは炎上しがちなので、それに対するナレッジとかそういうのを得るために読んだ。 |

Kindleとかで買って、特にやることがない電車移動中とかバスの中とかでテキトーに読むのがオススメです。僕はわざわざプライベートな時間に仕事のこととか勉強したくないです。 |

##  (参考2)React学習時に僕がよく見てたサイト
| タイトル | 感想 |
| ---- | ---- |
| [TypeScript入門『サバイバルTypeScript』 ](https://typescriptbook.jp/) | 参画した当初はReactもイミフでjavascriptアレルギー持ってたのもあり(フロントリーダーになった時は絶望した…)、最初いきなり全ては理解できなかったが、理解できるようになったら何度も読み返しては**頭に入ってこないものは後回し**にしてそのうちまた読み返す…というのを繰り返してたらなんか定着してた。 |
| [React Developer Roadmap: Learn to become a React developer ](https://roadmap.sh/react) | 自分がどのくらいのレベルに居るのか知るために見てた。使ったことがなくても、そういうライブラリがあるんだ～みたいな知識を軽く持っておくだけで技術選定が将来的にラクになると思う。ちなみに他言語版もあるっぽい。 <br> 例：https://roadmap.sh/php |
| [React リファレンス概要 – React ](https://ja.react.dev/reference/react) | あらゆるライブラリがそうなのですが、**とにかく一次情報を読むことが重要**です。これもよくわからないものは後回しにして、しばらく経ったら読み返して足りない知識をアップデートしてた。特にフック周り。<br>正直Reactは覚えることが少ないし、何より日本語のドキュメントがあるから学習は圧倒的にラクだった。|

こういうの勉強するのダルくて嫌いなんですが、知識があるとラクができるシーンが多々あるので、スキマ時間にちょろっと読むのがオススメ。仕事してて一番嫌いなバグフィクスという時間は、**知識があれば圧倒的に短縮できる。** |

(レビューする側について)
技術に関係ない話としては**「褒められる点があれば褒める・感想を伝える」**というのもレビューするにあたって大事かなと思います。自分は当時できなかったことなのですが、印象はだいぶ変わるかなと思います。
