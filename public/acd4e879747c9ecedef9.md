---
title: 【JavaScript】宝くじ (paizaランク C 相当) やってみたのと解説
tags:
  - JavaScript
  - paiza
  - paiza×Qiitaコラボキャンペーン
private: false
updated_at: '2024-07-20T20:50:06+09:00'
id: acd4e879747c9ecedef9
organization_url_name: null
slide: false
ignorePublish: false
---
これのやつです。

https://qiita.com/official-events/9ab96aa95d62fe3cbdd7

今回は、「どんな風に書いていこうかな」ってノリを模索するために書いてます。
書き方は他の人のを真似しています。

# 問題
https://paiza.jp/works/mondai/c_rank_skillcheck_archive/lottery

# 解答コード
```javascript
process.stdin.resume();
process.stdin.setEncoding('utf8');
// 自分の得意な言語で
// Let's チャレンジ！！
var lines = [];
var reader = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout
});
reader.on('line', (line) => {
  lines.push(line);
});
reader.on('close', () => {
    const winningNumber = Number(lines[0]);
    
    const adjacents = [];
    if(100000 < winningNumber) adjacents.push(String(winningNumber - 1));
    if(199999 > winningNumber) adjacents.push(String(winningNumber + 1));
    
    const n = Number(lines[1]);
    for(let i=0; i<n; i++){
        const currentNumber = lines[i+2];
        
        if(currentNumber === lines[0]){
            console.log("first");
        }else if(adjacents.includes(currentNumber)){
            console.log("adjacent");
        }else if(currentNumber.endsWith(lines[0].substr(-4))){
            console.log("second");
        }else if(currentNumber.endsWith(lines[0].substr(-3))){
            console.log("third");
        }else{
            console.log("blank");
        }
    }
    
});
```

# 解説
色つけと書き込みすぎて汚いかも
![CleanShot 2024-07-20 at 20.46.45@2x.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/435274/dc336fa0-d04d-c82b-6802-a1af4a21d48e.png)

# 感想
自分なりに解説書いてみたけど見にくいかも。
というか問題文のスクショって載せて大丈夫なのだろうか。
とりあえず、その時々でこんな感じでやってみる。
