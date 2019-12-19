
ドレス作成・運用ガイド（１）はじめに
================================================================================================================================




ドレスとは
--------------------------------------------------------------------------------------------------------------------------------

一般に「テーマ」や「スキン」と呼ばれるものを、ビビでは「ドレス」と呼んでいます。

アプリケーションの外観のうち、アプリケーションの基本的な振る舞いに関わる部分を除いたカスタマイズ可能な部分を外部に切り離しておけば、利用者は、安全な範囲でアプリケーションの外観をカスタマイズすることができるようになります。
同時に、そのカスタマイズ成果のみをアプリケーション本体とは独立して別の利用者に配布したり、逆に別の利用者が配布しているカスタマイズ成果を自分も利用できるようになります。

こうした「着せ替え」の仕組みが、一般的に「テーマ機能」「スキン機能」などと呼ばれるものです。また、アプリケーション本体と独立して交換可能なカスタマイズ定義ファイルは、「テーマ」「スキン」などと呼ばれています。そしてビビでは、これらを「ドレス機能」「ドレス」と呼ぶことにしました。




ドレスの実体は１点の CSS ファイル
--------------------------------------------------------------------------------------------------------------------------------

ビビの外観は、bibi/index.html から呼び出される２つのスタイルシートによって定義されています。

```
<link id="bibi-style" rel="stylesheet" href="resources/styles/bibi.css" />
<link id="bibi-dress" rel="stylesheet" href="wardrobe/everyday/bibi.dress.css" />
```

１つめの `<link id="bibi-style" rel="stylesheet" href="resources/styles/bibi.css" />` が呼び出している CSS ファイルは、動作やレイアウトの根幹に関わるもので、カスタマイズを推奨できない内容がまとめられています。この CSS の内容についてはここでは触れません。

２つめの `<link id="bibi-dress" rel="stylesheet" href="wardrobe/everyday/bibi.dress.css" />` が呼び出している CSS ファイルは、ボタンやスライダーといった UI を構成するパーツの色や大きさ、書籍の表示されるエリアの背景など、変更しても動作やレイアウトに影響を与えにくい一方で視覚的にアプリケーションの個性を表現するのに適したデザイン要素のみを切り離したものです。

２つめに挙げた１点の CSS ファイルが、ドレスの実体ファイル＝ドレスファイルです。

ドレスファイルは、これから説明する方法で作成することができます。また、自分で作ったドレスも、誰かが作ったドレスも、いくつでも追加しておくことができます。
そして、上の２点目のスタイルシート定義部分を変更することで、利用するドレスファイルを切り替え、好きなドレスに着せ替えることができます。




誰かが作成したドレスファイルを利用するには
--------------------------------------------------------------------------------------------------------------------------------

もし自分でドレスを作ろうとするのでなく、誰かが作ったドレスファイルに着せ替えたいだけの場合は、この項目以降の説明のほとんどは、読まなくても構いません。
逆に、自分でドレスをつくろうとする場合は、この項目を読み飛ばして次の項目に進んでください。

たとえば、次のような構成の special ドレスフォルダを誰かから受け取って、それに着せ替えたい、とします。

```
+ special
    - bibi.dress.css 
```

まず special フォルダを、次のように、サーバに設置したビビの wardrobe ディレクトリ内に設置（アップロード）してください。

```
+ bibi/
    + wardrobe/
        + special/
            - bibi.dress.css
```

あとは、このドキュメントの下のほうにある「ドレスの追加・変更　手順４．使用するドレスファイルの切り替え」の手順に従ってサーバ上の bibi/index.html 中の記述を更新するだけで、special ドレスに着せ替えることができます。
（special ドレスをサーバに直接設置するのではなく、開発環境に加えたい場合は、これ以降の項目も読んでください。）

誰かが作成した別のドレスをさらに追加する場合も、同様に bibi/wardrobe/ ディレクトリ下にアップロードします。あとは、`bibi/index.html` 内を書き換えれば、着せるドレスを自由に選ぶことができます。




ドレス作成に関する技術仕様と事前の注意事項
--------------------------------------------------------------------------------------------------------------------------------

ドレスのソースファイルは SCSS で記述されています。
したがって、自分であたらしいドレスを作成するには Sass と CSS に関する若干の知識が必要です。

ただし、ウェブデザイナーのようにゼロから CSS / SCSS を書くことができるほどの知識は必要ありません。
あらかじめテンプレートに変数として列挙してあるカラーコードやピクセルサイズを好きな値に変更するだけで、広範囲のカスタマイズが可能です。
より具体的には、「ドレス作成・運用ガイド（２）カスタマイズ編」（02_Customize.md）で概説します。

SCSS から CSS へのコンパイル機能（変換処理）は、ビビの開発環境にあらかじめ備わっています。
ターミナルからコマンドを１行実行するだけで、他のファイルと一緒に、ドレスの CSS ファイルも適切な場所に自動生成されます。

　＊

ドレス機能はまだ実装されたばかりです。また、ビビ本体も日々進化を続けています。そのため、作成したドレスがビビの将来のバージョンにわたって完全に有効であり続ける保証はできません。
ただし、閲覧者の読書の妨げになるような表示崩れや、カスタマイズ成果が完全に無駄になるような事態は、できるだけ抑えられるような設計にしてあります。ビビ本体の開発者も、ドレスの互換性ができるかぎり維持されるように努めます。
したがって、もし本体の変更に伴ってドレスの互換性が部分的に失われても、少ない修正で再度有効なドレスに仕立て直すことができるはずです。




あたらしいドレスの作成に関わるファイル・フォルダ
--------------------------------------------------------------------------------------------------------------------------------

次の構成図は、クローンしたままのビビの構成ファイル・フォルダのうち、ドレスの追加・変更に関係するファイル・フォルダだけを抜き出したものです。

```
+ __src
    + bibi/
        - index.html … 使用するドレスを決定する HTML ファイルです。
        + wardrobe/
            - _dresses.js … 用意しておきたいドレスの名前を列挙しておくファイルです。
```

配布段階でこの構成図に挙げられていないファイル・フォルダは、ドレスの新規作成には関係ないため、別の目的がないかぎり編集する必要はなく、以降の説明でも言及しません。




ドレスの追加・変更手順全体の概要
--------------------------------------------------------------------------------------------------------------------------------

※ この項目は、一度ドレス作りの概要を把握した後の振り返りを助けるために挟んであります。最初は読み飛ばして次に進むことを推奨します。

1. テンプレートフォルダを生成・リネームして、あらたなドレスのソースフォルダを作る
2. _dresses.js にドレス名を追加する
3. ビビ全体の生成処理を実行してドレスファイルを生成する
4. index.html で読み込むドレスファイルを変更する
5. 表示を確認しながらソースフォルダ内を編集してドレス作りを進める




ドレスの追加・変更　手順１．ドレスのソースフォルダを準備する
--------------------------------------------------------------------------------------------------------------------------------

まず、ターミナルでビビのリポジトリのディレクトリに移動してください。
それから、次のコマンドを実行します。

```
npm run make:dress-template
```

すると、さきに挙げた構成図にある wardrobe ディレクトリ（__src/bibi/wardrobe/）に、生成日時を含む DRESS-TEMPLATE-20191218-153045 のような名前のフォルダが生成されます。
これがこれから新しく作成するドレスのテンプレートになるフォルダで、中身にはドレスを構成するソースファイルのすべてが含まれています。

まずは、このフォルダを任意の名称に変えてください。ここでは仮に cocktail という名前にしておきます。
このフォルダをソースフォルダと呼びます。




ドレスの追加・変更　手順２．ドレスファイルを生成処理の対象に追加する
--------------------------------------------------------------------------------------------------------------------------------

用意したソースフォルダからドレスファイルを生成するためには、ビビ全体のファイル生成処理を利用します。

ビビ全体のファイル生成処理は、ドレス以外も含む __src 以下のソースファイルに基づき、実際にサーバに設置して使用できるファイルを __dist 以下に生成します。
__src/bibi/wardrobe/ にドレスのソースフォルダを設置し、__src/bibi/wardrobe/_dresses.js にドレス名を記載することで、ビビ全体のファイル生成処理を実行するときにそのドレスも処理対象となり、__dist/bibi/wardrobe/ 内にドレスフォルダ・ドレスファイルが生成されるようになります。

_dresses.js を開くと、次のような内容になっています。

```
module.exports = {
    'custom-made': [
        'everyday'
    ],
    'ready-made': [
    ]
};
```

everyday というドレスのみが生成処理の対象になっている状態です（everyday ドレスは、デフォルトの外観として用意されている特別なドレスです）。

これを次のように書き換えます。

```
module.exports = {
    'custom-made': [
        'cocktail',
        'everyday'
    ],
    'ready-made': [
    ]
};
```

'custom-made' に 'cocktail' を追記しました。
これで cocktail ドレスが生成処理の対象に加わります。

ドレス名は、１文字目が半角英数で、２文字目以降は半角英数・アンダースコア・ハイフンだけから成る必要があります。
この書式を充たさない場合、処理対象に加わることなく無視されます。

　＊

もしも誰かが作成したドレス（special とします）も使いつつ自分でもドレスをつくりたい場合は、まず、special ドレスフォルダを開発環境の wardrobe ディレクトリ（__src/bibi/wardrobe/）に設置してください。
次に、special ドレスフォルダ内が bibi.dress.scss ドレスソースファイルなら、cocktail と同じように _dresses.js の 'custom-made' に 'special' を追加します。

```
module.exports = {
    'custom-made': [
        'cocktail',
        'special',
        'everyday'
    ],
    'ready-made': [
    ]
};
```

こうすれば special ドレスも生成処理に加わります。
ただし、もし special ドレスフォルダ内が既に生成処理を経た bibi.dress.css ドレスファイルなら、次のように、_dresses.js の 'ready-made' に 'special' を加えてください。

```
module.exports = {
    'custom-made': [
        'cocktail',
        'everyday'
    ],
    'ready-made': [
        'special'
    ]
};
```

'ready-made' に加えられたドレスは、ファイル生成処理を実行すると、__src/bibi/wardrobe/ から __dist/bibi/wardrobe/ にそのまま複製されます。中身は一切処理されません。
__dist/bibi/wardrobe/ 内はファイル生成処理の際に 一旦すべて削除されてしまうため、既成ドレスは直接 __dist/bibi/wardrobe/ に置くのではなく、かならず __src/bibi/wardrobe/ に設置するようにしてください。

なお、'custom-made' と 'ready-made' に同じ名前のドレスがある場合、'ready-made' にのみ記述されたものとして扱われます。






ドレスの追加・変更　手順３．ドレスファイルの生成を確認
--------------------------------------------------------------------------------------------------------------------------------


先に書いたとおり、ドレスファイルの生成は、ビビ全体のファイル生成処理の中で行われます。

ターミナルで、さきほどテンプレートフォルダを生成したときからディレクトリを移動していなければそのまま、次のコマンドを実行してください。

```
npm run build
```

すると、サーバに設置するビビの構成ファイル群（__dist ディレクトリ以下）が生成され、その中には下の CSS ファイルも生成されているはずです。

```
+ __dist/
    + bibi/
        + wardrobe/
            + cocktail/
                - bibi.dress.css
```

この bibi.dress.css ファイルが、新しい cocktail ドレスのドレスファイルです。




ドレスの追加・変更　手順４．使用するドレスファイルの切り替え
--------------------------------------------------------------------------------------------------------------------------------

使用するドレスファイルは、この README の最初のほう（「ドレスの実体」の項）にあるとおり、bibi/index.html 内の記述で決まります。
__src 以下の開発環境の bibi/index.html は編集内容がそのまま __dist 以下に反映されますので、開発時には __src/bibi/index.html を編集します。

```
+ __src/
    + bibi/
        - index.html
```

この index.html を開くと、中程に次のような記述があります。

```
<link id="bibi-style" rel="stylesheet" href="resources/styles/bibi.css" />
<link id="bibi-dress" rel="stylesheet" href="wardrobe/everyday/bibi.dress.css" />
```

ここで２種のスタイルシートを連続して呼び出しているうち、後者がドレスです。
デフォルトでは everyday ドレスが使用されていますので、href 属性の値を次のように変更し、cocktail ドレスファイルを呼び出すようにします。
具体的には、href 属性値の中の everyday というディレクトリ名を cocktail に置き換えるだけです（cocktail ではない名前でドレスを作成した場合は、もちろんその名前に置き換えます）。

```
<link id="bibi-style" rel="stylesheet" href="resources/styles/bibi.css" />
<link id="bibi-dress" rel="stylesheet" href="wardrobe/cocktail/bibi.dress.css" />
```

ドレスの切り替えはこれだけで完了です。




ドレスの追加・変更　手順５．ドレスのカスタマイズ
--------------------------------------------------------------------------------------------------------------------------------

ここまでの段階では、まだ cocktail ドレスはカスタマイズされていないため、everyday ドレスと同じ見た目のままです。
ここから、cocktail ドレスを自由にカスタマイズしていきます。
具体的な方法は「ドレス作成・運用ガイド（２）カスタマイズ編」（02_Customize.md）を参照してください。

今後は index.html 内の記述を変えるだけで、いつでも everyday ドレスに戻したり、さらにまた cocktail ドレスにしたり、自由にドレスファイルを切り替えることができます。




参考）さらに別の自作ドレスを追加して、複数のドレスを管理する
--------------------------------------------------------------------------------------------------------------------------------

もしさらに別のドレスをあらたに作成するときは、cocktail ドレスを作ったときと同様に、まず `npm run make:dress-template` で作成したドレスソースフォルダを任意の名前に変更し、生成処理対象として _dresses.js にドレス名を追記します。

_dresses.js には、下のように順不同で追加することができます。

```
module.exports = {
    'custom-made': [
        'party',
        'cocktail',
        'everyday',
        'wedding'
    ],
    'ready-made': [
    ]
};
```

_dresses.js に列挙されたドレスは、すべて生成処理の対象になり、個別のドレスファイルが生成されます。
既に完成していたり不要になったりして都度の生成処理にかける必要がなくなったドレスは、_dresses.js 内でコメントアウトするなどして、生成処理の対象から外すことができます（everyday ドレスも例外ではなく、もし不要なら生成処理の対象から外しても構いません）。


