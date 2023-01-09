+++
title = "GASで成績判定表を作ってみよう！"
date = 2023-01-10T01:50:29+09:00
lastmod = true
author = "hayabusa1601"
tags = ["GAS"]
cover = ""
summary = "第一回勉強会用"
+++

# 勉強会の参考テキスト
## 目次
1. GASの基本のキ 
2. スプレッドシート操作の基本
3. 成績確認のシートを作ってみよう！

&nbsp;


## GASの基本、スプレッドシート操作の基本

[こちらを参照してください]({{< ref "/post/Jan10_GAS_cheatsheet.md" >}})

&nbsp;


## 成績確認のシートを作ってみよう！

### 概要
以下の機能を実装してみる。
- 教科ごとの点数を入力すると、赤点かそうでないか判別できる。
- 進級までの残り点数を見ることができる。
- 確認ボタンでApps Scriptを開かずとも実行できる 

&nbsp;


### 準備
すでに作成されているスプレッドシートで新しいシートを作る、もしくは新しくスプレッドシートのファイルを作成してApps Scriptを作成するか、どちらかを行って新しい関数 checkGrade()を作成する。今後はこの中に処理を記入していこう。

```gas
function checkGrade() {
 //ここに記入していく
}
```

「実行」「デバッグ」の横にあるmyFunctionを変更して、実行する関数を変更することができる！ここでcheckGrade()を指定しておこう。
{{< figure src="/images/post/Jan10_GAS_spreadsheet/checkGrade.png">}}


&nbsp;


### ファイル・シートを開く
まずはファイルを開いてシートを指定しよう
```
 const ID = 'hogehoge';
 let densan = SpreadsheetApp.openById(ID);
 
 //名前からシートを開く
 const sheet_name = '成績判定';
 let sheet2 = densan.getSheetByName(sheet_name);
```

ファイルidとシート名を間違えないようしよう。ファイルidが一文字でも間違っていれば開けないし、シート名の半角全角は間違いやすい。

&nbsp;


### 枠を書く
次に枠を書こう。行である教科名はいくつ書いても大丈夫な実装をする予定だ。弊学生向けの資料であるから、テスト四回分と結果と進級までの残り点数の列を作成している。 

{{< figure src="/images/post/Jan10_GAS_spreadsheet/waku.png">}}


&nbsp;


### 結果を判定していく
このプログラムは、主に一行の実装を、値が入っている最終行までfor文で回すことで結果を埋めることができる。
```
let lastrow = sheet2.getLastRow(); //最終行を取得

//最終行まで回す
for (let i = 2; i <= lastrow; i++) {

}
```

自分の場合、最初のセルはB2であるから、forの初期値は2にすればよい。  

&nbsp;


つぎに点数を合計していこう。  

2～5列の値を取得して合計することで、合計点数を求めることができるため、for文をネストして2～5列目の値を合計する処理を書く。  
row_scoreを0で定義し、値を読み取るごとにその変数に足していけば合計できる！  
ついでに平均点も計算しておく。

```
for (let i = 2; i <= lastrow; i++) {
	let row_score = 0; // 合計点数
	let average_score;

	for (let j = 2; j <= 5; j++) {
		//合計に加える
		row_score += sheet2.getRange(i,j).getValue();
	}
	average_score  = row_score / 4;
}
```

for文の処理が完全に終了してから計算を行うため、average_scoreは内側のfor文の外に定義する必要がある。  

&nbsp;

平均点から、結果のセルに代入する値を設定していこう。if文による条件分岐で青点、赤点、黒点いずれかを表示させていく。  
  
変数average_scoreが30未満であれば青点、60未満であれば赤点、100未満であれば黒点、それ以外では未入力という条件で設定した。私のシートでは結果は6行目であるから、結果の行をiに列を6にしている。   

ついでにセルの背景の色も変更しておこう。setBackground()でカラーコードを指定することでセルの色を変更することができる。

以下は全体のコードである。

```
for (let i = 2; i <= lastrow; i++) {
	let row_score = 0; // 合計点数
	let average_score;

	for (let j = 2; j <= 5; j++) {
		//合計に加える
		row_score += sheet2.getRange(i,j).getValue();
	}
	average_score  = row_score / 4;

    if (average_score < 30) {
      sheet2.getRange(i,6).setValue("青点！");
      sheet2.getRange(i,6).setBackground("#00ffff");

    } else if (average_score < 60) {
      sheet2.getRange(i,6).setValue("赤点！");
      sheet2.getRange(i,6).setBackground("#ff0000");

    } else if (average_score <= 100) {
      sheet2.getRange(i,6).setValue("黒点！");
      sheet2.getRange(i,6).setBackground("#00ff00");

    } else {
      sheet2.getRange(i,6).setValue("未入力");
      sheet2.getRange(i,6).setBackground("#ffffff");

    }
}
```

&nbsp;


### 表が未完成でも動作するようにする
とりあえず最低限動作するようになった！　しかし、今回は行に空白がある＝まだ表が完成していない段階でも赤点か否かを判定できるようにしてみる。  

空白があるか否かの判定はisBlank()を使用する。空白が存在する場合、平均点の計算がおかしくなるため、空白の判定のif文の内部で平均点を処理する。今回は空白がみつかる前までの値だけを使って、その時点での平均値を求めている。    

空白が存在しない場合には合計点数に加算して、合計後に平均点を出す。  


```
for (let i = 2; i <= lastrow; i++) {
	let row_score = 0; // 合計点数
	let full_flag = true; //空白があるかのフラグ
	let average_score;

	for (let j = 2; j <= 5; j++) {
		if (sheet2.getRange(i,j).isBlank() == true) {
			//空白であればfalseにしてループから抜ける。
			full_flag = false;

			//この時点での平均点を計算 
			average_score = row_score / (j-2);
			break;
			
		} else {
		//合計に加える
		row_score += sheet2.getRange(i,j).getValue();
		
		}
	}

	//空白がない場合は4で割る
    if (full_flag == true) {
      average_score = row_score / 4;

    }
}
```
&nbsp;



### 残り点数を表示させる
もうすぐでひとまず完成だ。テスト点数の合計がひと教科あたり合計で、240点ほどあれば進級できる。この240点までの点数を表示させてみよう。   

まずは定数としてBORDER_LINEを240で定義する。  

次に、空白が存在する場合としない場合の処理に、BORDER_LINEとその時点の合計点row_scoreの差next_scoreを計算する。

その値をシートの7列目に表示させればよい。

```
const BORDER_LINE = 240;

for (let i = 2; i <= lastrow; i++) {
	let row_score = 0 // 合計点数
	let full_flag = true; //空白があるかのフラグ
	let average_score;

	for (let j = 2; j <= 5; j++) {
		if (sheet2.getRange(i,j).isBlank() == true) {
			//空白であればfalseにしてループから抜ける。
			full_flag = false;

	        //この時点での点数を返す
	        let next_score = BORDER_LINE - row_score;
	        sheet2.getRange(i,7).setValue(next_score);

	        //この時点での平均点を計算
	        average_score = row_score / (j-2);

			break;
			
		} else {
		//合計に加える
		row_score += sheet2.getRange(i,j).getValue();
		
		}
	}

	//空白がない場合は4で割る
    if (full_flag == true) {
      average_score = row_score / 4;
      let next_score = BORDER_LINE - row_score;
      sheet2.getRange(i,7).setValue(next_score);

    }
}
```

&nbsp;

{{< figure src="/images/post/Jan10_GAS_spreadsheet/hitomazu.png">}}


### ボタンで確認
最後に、この一連の動作の関数をスプレッドシート側からボタンひとつで動作するようにしてみよう。  

挿入→図形描画で図形を描画して、ボタン風に文字を入れてみる。

{{< figure src="/images/post/Jan10_GAS_spreadsheet/button1.png">}}

{{< figure src="/images/post/Jan10_GAS_spreadsheet/button2.png">}}

図形から、「スクリプト割り当て」を選択して、この動作の関数である「checkGrade()」を選択する。

{{< figure src="/images/post/Jan10_GAS_spreadsheet/button3.png">}}


これでひとまず完成！

{{< figure src="/images/post/Jan10_GAS_spreadsheet/button4.png">}}

&nbsp;


### 完成したコード
```
function checkGrade() {
  const BORDER_LINE = 240;
  const ID = 'hogehoge';
  let densan = SpreadsheetApp.openById(ID);

  //名前からシートを開く
  const sheet_name = '成績判定';
  let sheet2 = densan.getSheetByName(sheet_name);

  //値の入っている最終行を取得
  let lastrow = sheet2.getLastRow();

  //結果判定
  //一行ずつ実行
  for (let i = 2; i <= lastrow; i++) {
    let row_score = 0; //合計点数
    let full_flag = true; //空白があるか判定
    let average_score;

    //点数の計算
    for (let j = 2; j <= 5; j++) {
      if (sheet2.getRange(i,j).isBlank() == true) {
        //空白が一つでもあったらfalse
        full_flag = false;  

        //この時点での点数を返す
        let next_score = BORDER_LINE - row_score;
        sheet2.getRange(i,7).setValue(next_score);

        //この時点での平均点を計算
        average_score = row_score / (j-2);

        //このループを抜ける
        break;
      } else {
        //空白でなければ合計に加える
        row_score += sheet2.getRange(i,j).getValue();

      }
    }  


    if (full_flag == true) {
      average_score = row_score / 4;
      let next_score = BORDER_LINE - row_score;
      sheet2.getRange(i,7).setValue(next_score);

    }

    Logger.log("合計点数は" + row_score);
    Logger.log("平均点は" + average_score);

    if (average_score < 30) {
      sheet2.getRange(i,6).setValue("青点！");
      sheet2.getRange(i,6).setBackground("#00ffff");

    } else if (average_score < 60) {
      sheet2.getRange(i,6).setValue("赤点！");
      sheet2.getRange(i,6).setBackground("#ff0000");

    } else if (average_score <= 100) {
      sheet2.getRange(i,6).setValue("黒点！");
      sheet2.getRange(i,6).setBackground("#00ff00");

    } else {
      sheet2.getRange(i,6).setValue("未入力");
      sheet2.getRange(i,6).setBackground("#ffffff");

    }

  }

}
```

&nbsp;


### 展望
今後は、総計で留年したか進級したか等の判定等の機能を実装してみるといいかも