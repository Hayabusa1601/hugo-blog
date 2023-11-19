+++
title = 'GASの基本メモ'
date = 2023-01-10T01:39:53+09:00
lastmod = 2023-01-10T22:39:53+09:00
categories = ['GAS']
tags = ["GAS", "cheatsheet"]
draft = false
#thumbnail = "img/placeholder.png" # サムネ
lead = "cheatsheet" # ハイライトされたタイトルの近くのテキスト
comments = false # Disqus用のコメントページ　Disqus未導入のためfalse
pager = true # prev/nextナビを有効に

+++


---
 ## 参考文献
公式リファレンス
 [Reference overview  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/reference?hl=en)
 qiita
[GoogleAppsScriptのチートシート - Qiita](https://qiita.com/sahksas/items/e08996327b18df27b27e)
  
&nbsp;&nbsp;  

---
## 使い方
ここから「新規スクリプト」で新たに作成できる。
https://script.google.com

関数myfunctionがあらかじめ定義されているので、その中に記入していく。
&nbsp;
&nbsp;


---
## 基本的な動作
### ログの出力
まずはHello worldしてみよう。

```
function log_test() {
 Logger.log('Hello world!');
}
```
Cでいうところのprintf。実行ログに出力される。
値がいまどうなってるかをここで確認することができる。
&nbsp;

### 変数宣言
```
var hensu; //var
let hensu2; //let
const hensu3 = 'こんにちは' //const
hensu = 'Hello'!;

hensu2 = 100;
Logger.log(hensu);
Logger.log(hensu2);
```

基本的にはvarではなくlet使ったほうがいい。
&nbsp;
変数として利用可能なデータ型は、以下が存在する
- 数値型(Number)
- 文字列型(String)
- 真偽型(Boolean)
- undefind
- null
- 配列(Array)
- オブジェクト(Object)
- 日時型(Date)

&nbsp;
&nbsp;

typeof関数を使用することで、データ型を調べることができる
```
typeof(hensu);
```

変数の定義方式は以下の種類があり、それぞれ違う性質を持つ

| 定義方式 | 定義/変数  | もう一度代入 | 再宣言 | スコープ |
| -------- | ---------- | ------------ | ------ | -------- |
| var      | 変数を宣言 | 可能         | 可能   | ローカル |
| let      | 変数を宣言 | 可能         | 不可能 | ブロック |
| const    | 定数を宣言 | 不可能       | 不可能 | ブロック |

&nbsp;

### 配列
```
let array_1 = ['Red','Blue', 'Yellow'];
let array_2 = [10,20,30];
```
最初の要素が0で、1、２……と増えていく。

&nbsp;

### 文字列の操作
#### 連結
```
let str_1 = 'こんにちは。';
let str_2 = '私は熊本高専生です。';
let str_link = str_1 + str_2;
Logger.log(str_link); //こんにちは。私は熊本高専生です。
```
文字を+演算子で結合することができる。

&nbsp;

#### 置き換え
```
let str_3 = '私はお米が好きです';
let str_replace = str_3.replace('米', '金');
Logger.log(str_replace);
```
replace()を使用することで、文字列の特定の文字を違う文字に置き換えることができる。

&nbsp;

---
## スプレッドシートの操作

スプレッドシートから操作することもできる 
Google driveからスプレッドシートを新規作成→スプレッドシート操作画面で「拡張機能」を選択→Apps Scriptを選択

&nbsp;

### スプレッドシートを開く
```
const id = 'aiueokstnhmyrwn'; //ファイルidを代入
let file = SpreadsheetApp.openById(id);
```

ファイルidとは、URLの内部に存在する。以下の部分に存在する。
https://docs.google.com/spreadsheets/d/ファイルid/edit#gid=シートid

&nbsp;

### シートを開く
```
 //名前からシートを開く
  const sheet_name = 'シート1'
  let sheet1 = file.getSheetByName(sheet_name);
```
ファイルだけでなくシートを指定する必要がある。ファイルidからgetSheetByName()を使用して名前からシートを開くことができる。シートは下部のバーから追加・変更できる。

&nbsp;

### セルの値を取得する
```
  //セルの値を取得する
  let row = 2, //2行目
      col = 3; //3列目 (C列)
    
  //C2のデータを取得
  let c2_value = sheet1.getRange(row,col).getValue();
  Logger.log(c2_value);
```
受け取ったシート内でgetRange()で行と列を指定し、getValue()で値を取得できる。

&nbsp;


```
  //複数の値の取得
  let row2 = 5, //~5行
      col2 = 3; //~3列
 
  //C2:E6の値を取得
  let cell_values = sheet1.getRange(row,col,row2,col2).getValues();
  Logger.log(cell_values)
```
複数の値を取得する場合は、getValue()ではなくgetValues()を使用する。
getRange()に指定する値を増やすことで、複数範囲の値を取得できる。このサンプルコードの場合は2~6行、3~5列の5行3列の値を取得している。

&nbsp;

### セルの値を設定、変更する
```
sheet1.getRange(row,col).setValue('こんにちは');
```
setValueを使用する。複数のセルをまとめて設定することはできない。

&nbsp;

### ブランク判定
```
  if (sheet1.getRange(row,col).isBlank() == true) {
    Logger.log('ブランクです');
  } else {
    Logger.log('値が入っています');
  }
```

値が入っているかはisBlank()を使用して判定でき、空白の場合はtrueを返して値が入っている場合はfalseを返す。

&nbsp;

### 最終行・最終列を取得
```
  //セルに入力のある最後の行・列を取得
  let lastrow = sheet1.getLastRow(); 
  let lastcol = sheet1.getLastColumn();

  Logger.log(lastrow);
  Logger.log(lastcol);
```

セルに入力してある最大の行もしくは列を取得する。

&nbsp;

### 行の挿入
```
 sheet1.insertRowAfter(2) //2行目の後に一行挿入
 sheet1.insertRowBefore(5) //5行目の前に一行挿入
 sheet1.insertRowsAfter(10,2) //10行目の後に二行挿入
 sheet1.insertRowsBefore(15, 3) //15行目の前に三行挿入
```

RowをColumnに変更することで、列でも同じことができる。（行は英語でRow、列は英語でColumn)

&nbsp;

### 行の削除
```
  sheet1.deleteRow(3) //三行目を削除
  sheet1.deleteRows(20,6) //20行目～25行目を削除
```

こちらもRowをColumnに変更することで、列でも同じことができる。（行は英語でRow、列は英語でColumn)

&nbsp;
