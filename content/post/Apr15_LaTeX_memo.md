+++
title = "【LaTeX】超個人主観で管理されたコピペ用テンプレート"
date = 2022-04-15T21:52:24+09:00
lastmod = true
author = "ハヤブサ"
tags = ["LaTeX"]
cover = "　"
summary = "　"
+++

# はじめに
こんにちは。ハヤブサです。いかがお過ごしでしょうか。私は来週からドキドキ！地獄のレポート生活が始まります。

私は昨年からCloud LaTeXを使用しTeXでレポートを書いています。昨年は~~~めんどくさいから~~~勉強のためにコマンドを忘れたら逐一調べながら作成していましたが、その作業を行うのはかなり億劫なのでレポートのテンプレートを作成しました。

現状、テンプレートは私が書いた我が子のようなクソレポとともに、GitHubのPrivateリポジトリで管理しています。GitHubであたらしくPublicリポジトリを作成して、テンプレートだけ公開するのもいいかなとは思ったのですが、どうせなら自分の備忘録として遺したいなと思い、こちらで公開することにしました。私は雰囲気に委ねてTeXを書いているので、アドバイスや改善案等がありましたら、笑顔でTwitterのDMに連絡お願いします。


Cloud LaTeXを使用しているので、環境構築のハナシとかはよくわかりません。

## ===追記=== 
**ホワイトモードだとなんか文字が白くて見えないので右上のボタンからダークモードにしてください**


# 目次
- [はじめに](#はじめに)
  - [===追記===](#追記)
- [目次](#目次)
- [テンプレート](#テンプレート)
  - [プリアンブル](#プリアンブル)
  - [図](#図)
    - [1枚のみ](#1枚のみ)
    - [２枚横に並べる](#２枚横に並べる)
  - [表](#表)
    - [1枚のみ](#1枚のみ-1)
    - [2枚横に並べる](#2枚横に並べる)
  - [ソースコード](#ソースコード)
    - [外部ファイル](#外部ファイル)
    - [直書き](#直書き)
  - [数式](#数式)
    - [記号](#記号)
      - [参考](#参考)
    - [SI単位系](#si単位系)
      - [マクロ](#マクロ)
      - [参考](#参考-1)
    - [入力](#入力)
      - [添字](#添字)
      - [分数](#分数)
      - [根号](#根号)
    - [文章中に入力](#文章中に入力)
    - [参考](#参考-2)
  - [URL](#url)
    - [参考](#参考-3)
- [箇条書き](#箇条書き)
  - [記号付き箇条書き](#記号付き箇条書き)
  - [番号付き箇条書き](#番号付き箇条書き)
  - [見出し付き箇条書き](#見出し付き箇条書き)
  - [参考](#参考-4)
- [Tikz](#tikz)
  - [参考](#参考-5)
- [参考文献](#参考文献)
- [おわりに](#おわりに)



# テンプレート
表はキャプションを上に、図はキャプションを下に記入する。

## プリアンブル
```tex
\documentclass[a4paper,10pt]{bxjsarticle}
\usepackage{xltxtra}
\usepackage{zxjatype}
\usepackage{here}

%数式環境最強
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{booktabs}
\usepackage{siunitx}%SI記号を使うため
\usepackage{bm}%ベクトルを表示するため
\usepackage{comment}%複数行のコメントを使用するため


%ここからソースコードの設定====================
\usepackage{listings} % listingを使うため
\renewcommand{\lstlistingname}{ソースコード}%キャプション名を「ソースコード」に変更

\lstset{
  language={C},%言語の設定
  basicstyle={\scriptsize},
  frame={tb},%上下にフレーム
}
%ここまでソースコードの設定====================


%TeXで回路図、フローチャートを書くため
\usepackage{tikz}
\usepackage{circuitikz}
\usetikzlibrary{shapes.geometric}
\usetikzlibrary{shapes.misc}

\usepackage{pdfpages}%pdfの入力をするため
\usepackage{url}%参考文献のurlを入力するため

\setjamainfont[BoldFont=ipaexm.ttf]{ipaexm.ttf}
\setjasansfont[BoldFont=ipaexg.ttf]{ipaexg.ttf}

\usepackage{subfigure}%図表を横に並べるため



%カウンタの操作をするため
\makeatletter
\newcommand{\figcaption}[1]{\def\@captype{figure}\caption{#1}}
\newcommand{\tblcaption}[1]{\def\@captype{table}\caption{#1}}
\makeatother


%ここから、段落、箇条書きの番号設定=========
%enum環境の一段目に「(1),(2),(3)...」と設定
\renewcommand{\labelenumi}{(\arabic{enumi}).}

%subsubsection段落をローマ数字に設定
\renewcommand{\thesubsubsection}{(\roman{subsubsection})}
%===========================================


%tikzの設定=================================
%端子(両丸の四角形)
  \tikzset{Terminal/.style={rounded rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
%プロセス(長方形)
    \tikzset{Process/.style={rectangle,  draw,  text centered, text width=3cm, minimum height=1.5cm}};
%ひし形
    \tikzset{Decision/.style={diamond,  draw,  text centered, aspect=3,text width=5cm, minimum height=1.5cm}};
%使用しないときはコメントアウトしないとエラー吐く
%=============
```


---
## 図 
### 1枚のみ
```tex
\begin{figure}[H]
\centering
\includegraphics[width=6cm]{}
\caption{}
\label{}
\end{figure}

```

### ２枚横に並べる
```tex
\begin{figure}[htbp]
	\begin{tabular}{cc}
	\begin{minipage}{.48\textwidth}
		\centering
        \includegraphics[width=6cm,height=6cm]{}
        \caption{}
        \label{}
	\end{minipage}
		\begin{minipage}{.48\textwidth}
		\centering
        \includegraphics[width=6cm,height=6cm]{}
        \caption{}
        \label{}
	\end{minipage}
	\end{tabular}
\end{figure}
```

---
## 表
[csv, Excel表→LaTeX表変換サイト](http://rra.yahansugi.com/scriptapplet/csv2tabular/)

このサイトつかってエクセルから変換する

### 1枚のみ
```tex

\begin{figure}[H]
\centering
\begin{tabular}{c|cc|cc|cc}
\hline

%ここに表を入力


\hline
\end{tabular}
\caption{ }
\end{figure}
```

---
### 2枚横に並べる
```tex

\begin{figure}[H]
\begin{tabular}{cc}


\begin{minipage}{.45\textwidth}
\centering

\begin{tabular}{ccc}

%ここに表を入力

\end{tabular}
\tblcaption{}
\end{minipage}
\hfill


\begin{minipage}{.45\textwidth}
\centering

\begin{tabular}{ccc}

%ここに表を入力

\end{tabular}
\tblcaption{}
\end{minipage}


\end{tabular}
\caption{}
\end{figure}

```

---
## ソースコード
```tex 
\lstset{}
```
内の設定を弄れば言語とか文字サイズとかフレームの有無の設定ができる。


### 外部ファイル
```tex
\lstinputlisting[caption=, label,]{}
```

### 直書き
```tex
\begin{lstlisting}[caption=,label]

\end{lstlisting}
```

---
## 数式
### 記号
くっそ多いのでよく使うものだけ
| 出力       | 入力     |
| ---------- | -------- |
| $$\alpha$$   | \alpha   |
| $$\beta$$    | \beta    |
| $$\epsilon$$ | \epsilon |
| $$\lambda$$  | \lambda  |
| $$\sum$$     | \sum     |
| $$\times$$   | \times   |
| $$\simeq$$   | \simeq   |
| $$\infty$$   | \infty   |
| $$\int$$     | \int     |
| $$\ldots$$   | \ldots   |
| $$\cdots$$   | \cdots   |




#### 参考
[LaTeXの特殊文字・特別記号](http://www.ic.daito.ac.jp/~mizutani/tex/special_characters.html)

### SI単位系
siunitxパッケージを読み込むことで使用できる。

コマンドは2種類存在し、単位のみの場合は小文字のsiコマンドを、単位と数値をあわせてsiパッケージで表示するときは大文字のSIコマンドを使用する。

```tex
\si{}
\SI
```

#### マクロ
多すぎ、一部のみメモ。
\si{}内に記入することで使用できる。
メートルとかモルとかは普通にアルファベット表記のほうがラクだけどね

| コマンド       | 単位           | 出力  |
| -------------- | -------------- | ----- |
| \ampere        | アンペア       | $$A$$   |
| \kelvin        | ケルビン       | $$K$$   |
| \kilogram      | キログラム     | $$kg$$  |
| \metre         | メートル       | $$m$$   |
| \mole          | モル           | $$mol$$ |
| \degreeCelsius | セルシウス温度 | ℃     |
| \coulomb       | クーロン       | $$C$$   |
| \hertz         | ヘルツ         | $$Hz$$  |

#### 参考
[SI単位（国際単位系） - siunitxパッケージのマクロ](https://medemanabu.net/latex/siunitx-macro/)



### 入力
数式の入力はequation内で入力する。
```tex
\begin{equation}
	f(x) = ax + b
	\label{}
\end{equation}
```

数式番号なしは\*を追加する
```tex
\begin{equation*}
	
\end{equation*}
```

#### 添字
わりと直感的に書ける

```tex
%上付き文字=========
 x^2 
 x^{2}
 x^{\prime}
 x^{2 + i}

%下付き文字===========
 x_i
 x_{i}
 x_{\epsilon}
 
```

#### 分数
```tex
 \frac{1}{2} %\frac{分子}{分母}
```

#### 根号

```tex
\sqrt{3}{27} %\sqrt{n乗根}{数式}
```



### 文章中に入力
\$と\$で囲えば数式入力できる。


### 参考
[LaTeX/LaTeXによる文書整形の応用/数式を記述する](https://cns-guide.sfc.keio.ac.jp/2001/11/4/1.html#SECTION012411000000000000000)

---
## URL
URLパッケージを使って\url使おう！！！


```tex
\url{}　%{}内にURL入力
```
### 参考
[LaTeXにURLを貼る時は必ず\url使う](https://tm23forest.com/contents/latex-url-paste)


# 箇条書き
箇条書きには3種類ある。

## 記号付き箇条書き
itemize環境で行う。アイテムの記述にはitemを使う。

また、記号を変更することもできる。その場合は\item [記号（文字）]と記述する。

```tex

\begin{itemize}
  \item 第一要素
  \item 第二要素
  \item 第三要素
  \item[$P \neq NP$] 第四要素
  \item[文字もおｋ] 第五要素
\end{itemize}

```

## 番号付き箇条書き
箇条書きで並べるときに、数字をつける。enumerate環境で行う。

```tex
\begin{enumerate}
  \item 第一要素
  \item 第二要素
  \item 第三要素
\end{enumerate}
```

## 見出し付き箇条書き
アイテムごとに見出しをつける。description環境で行う。

```tex
\begin{description}
  \item[第一要素] だいいちようそ
  \item[第二要素] だいにようそ
  \item[第三要素] だいさんようそ
\end{description}
```

プリアンブルにある設定をみるとrenewcommandの一文があるが、これは箇条書きルールを自分で定義している。

## 参考
[enumerate 環境の箇条書きを、括弧付きにしたり英語にしたり](https://joker.hatenablog.com/entry/20120114/1326488752)

[LaTeX 箇条書き](http://www.yamamo10.jp/yamamoto/comp/latex/make_doc/item/item.php#ITEMIZE)

# Tikz
ﾜｽﾚﾁｬｯﾀ……！

## 参考
[TikZでフローチャートを書く](https://molina.jp/blog/tikz%E3%81%A6%E3%83%95%E3%83%AD%E3%83%BC%E3%83%81%E3%83%A3%E3%83%BC%E3%83%88%E3%82%92%E6%9B%B8%E3%81%8F/)

[CircuiTikz package](https://www.overleaf.com/learn/latex/CircuiTikz_package)


# 参考文献

[理系大学生のための超手抜きLaTeXレポート入門](https://shiba6v.hatenablog.com/entry/2017/10/26/172825)

[イショティハドゥスにキレられないための LaTeX 論文執筆メソッド](https://qiita.com/Ishotihadus/items/bbbb85f54e6a4e7aaac0#disclaimer)

[アカデミックヤクザにキレられないためのLaTeX論文執筆メソッド](https://qiita.com/suigin/items/10960e516f2d44f6b6de)

[LaTeX 文書作成入門 【プリアンブル編】](https://blog.loliver.net/2020/08/10/latex-tutorial-preamble/)

[TeXで図表を横並びにする方法](http://takuma-art.blogspot.com/2011/07/tex.html)

[Cloud LaTeX](https://cloudlatex.io/projects)


# おわりに

LaTeXでかなりレポートを作成する際のイライラをなくすことができるので、みなさんも早くこちら側に来ることをおすすめします……
