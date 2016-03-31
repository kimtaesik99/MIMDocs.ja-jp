---
タイトル: Privileged Access Management の展開に関する考慮事項
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: dd883415-d39f-4903-8b01-1c8b52e8d447
---
# Privileged Access Management の展開に関する考慮事項
CAP Markdown エディターへようこそ!
====

キャップが最も一般的なマークダウン フレーバーの 1 つである GitHub フレーバー Markdown(GFM) を使用しています。 次のルールを使用してカスタマイズされた文書を作成する方法について説明します。  

*ビュー、 [マークダウンの基礎](https://help.github.com/articles/markdown-basics/)*<br/>
*ビュー、 [Github マークダウンのフレーバー](https://help.github.com/articles/github-flavored-markdown/)*<br/>
*ビュー、 [オンライン サンプル](http://github.github.com/github-flavored-markdown/sample_content.html)します。*<br/>
  
<br/>
クイック ガイド 
---
Markdown コンテンツを CAP を作成する際は、さまざまな方法からヘルプを取得でした。
- 使用 `Alt + F1` ie または `F1` を開くには Chrome で `Command Palette`
- 使用 `Ctrl + space` を開くには任意の場所に挿入します。
- コンテンツを作成するために、エディターの上部にあるオーサリング ツール バーを使用してください。

<br/>
コンテンツの書式を設定します。
---
- **段落**
  マークダウンの段落は、1 つ以上の空白行の後に連続するテキストの 1 つまたは複数の行です。
  
  ```
  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
  ```
  
- **見出し**  
  いずれかをヘッダー ドロップダウン リスト、オーサリング ツールバーまたは手動で提供されることができます追加 '---' または '= = =' セクション タイトルの下にある次の行にします。 1 つまたは複数追加することで、見出しに作成することもできます `#` 見出しテキストの前にシンボルです。 数 `#` を使用する見出しのサイズを決定します。

  ```
  # The largest heading (an <h1> tag)
  ## The second largest heading (an <h2> tag)
  …
  ###### The 6th largest heading (an <h6> tag)
  ```

- **ブロックの引用符**
  ブロックの引用符で囲みを示すことができます、 `>`です。
  ```
  In the words of Abraham Lincoln:
  
  > Pardon my French
  > Second line of the quote
  ```

- **テキストのスタイル設定**
  テキストを行うことができます **太字** または *斜体*
  ```
  **This text will be bold**
  *This text will be italic*  
  ```
  これを実現するのにオーサリング ツールバーを使用することもできます。

- **一覧表示します。**
  記号付きリストを追加するを持つリスト項目の前に `*` または `-`
  ```
  * Item
  * Item
  * Item
  
  - Item
  - Item
  - Item  
  ```
  順序付きリストを追加するには、数値を持つリスト項目の前に
  ```
  1. Item 1
  2. Item 2
  3. Item 3  
  ```
  入れ子になったリストを作成するには、2 つのスペースでリスト項目のインデントします。
  ```
  1. Item 1
    1. A corollary to the above item.
    2. Yet another point to consider.
  2. Item 2
    * A corollary that does not need to be ordered.
      * This is indented four spaces, because it's two spaces further than the item above.
      * You might want to consider making a new list.
  3. Item 3 
  ```
  これを実現するのにオーサリング ツールバーを使用することもできます。
  
- **コードの書式設定**
  三重タイマー刻みを使用することができます (' ') に独自の個別のブロックとテキストの書式設定します。
  この便利なプログラムを作成してください。  
  ~~~~
  ```
  x = 0
  x = 2 + 2
  what is x
  ```
  ~~~~

<br/>
コンテンツへの参照を追加します。
---
- **リンクを挿入します。**
  インライン リンクを作成するには角かっこでリンク テキストをラップすることによって `[Link Label]`, 、かっこ内のリンクをラップして `(http://URL)`します。 
  [Bing.com](http://www.bing.com)
  
  オーサリングのツールバーで、大文字で既存のトピックへのリンクを作成することもできます。
  
- **イメージを挿入します。**
  外部リソースからオンラインのイメージを参照するにを使用して `![Image Label]`, 、かっこで囲まれた画像リソースの url をラップ `(http://ImageURL)`
  
  オーサリングのツールバーで CAP からイメージを挿入することもできます。


<br/>
コンテンツ内のテーブルを使用します。
---
語の一覧をまとめて、(最初の行)、ハイフン - に分割することと、パイプの各列を分離してテーブルを作成することができます |。

最初のヘッダー  | 2 番目のヘッダー
------------- | -------------
セルのコンテンツ  | セルのコンテンツ
セルのコンテンツ  | セルのコンテンツ

インライン リンク、太字、斜体などのマークダウン形式の構文で追加または取り消し線を引くもできます。

| 名前 | 説明          |
| ------------- | ----------- |
| ヘルプ      | ~~表示、~~ ヘルプ ウィンドウです。|
| 閉じる     | _閉じる_ ウィンドウ     |

コロンを含めることによって複数の書式設定コントロールを持つことができます: ヘッダー行の中では、左揃え、右揃え、または中央揃えするテキストを定義することができます。

| 左揃え  | 中央揃え  | 右揃え |
| :------------ |:---------------:| -----:|
| col 3 は      | 冗長なテキスト | $1600 |
| col 2 は      | 中央揃え        |   $12 |
| シマ | きちんと        |    $1 |

コロン、 **一番左の** 側では、左側に整列させることを示します列; にコロン、 **右端の** 側では、右側に整列させることを示します列; にコロン **両方** 辺を中央揃えの列を示します。


<br/>
HTML
---
コンテンツ内で HTML のサブセットを使用することができます。 
当社のサポートされているタグと属性の完全な一覧が見つかります [ここで](https://github.com/github/markup/tree/master#html-sanitization)


  


<!--HONumber=Mar16_HO1-->


