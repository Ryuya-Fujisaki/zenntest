---
title: "TypeScript リテラル型おさらい"
emoji: "👻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["TS"]
published: true
published_at: 2025-03-20 14:45 # 未来の日時を指定する
---
```TS:TSLearning1.md
function compare(a: number, b: number): -1|0|1 {
    return a===b ? 1 : a>b ? 0 : -1
}

let execute = compare(6, 5)
console.log(execute)
```

※compare関数は、number型のaと、number型のbを引数に取り、-1または0または1を返す。
return以降三項演算子。a=bであれば、1を返す。そうでなければ、a>bであれば、0を返し、それ以外なら-1を返す。
変数executeは、6と5を引数に取るcompare関数。
executeをコンソールログ関数で実行。結果は0が返ってくる。