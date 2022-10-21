---
title: 'Reactのレンダリングについてgitの仕組みを交えながらわかりやすく説明する'
emoji: '👏'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [react, git, javascript]
published: true
---

# まず、React とはなにか？

React ではページの一部（例えばヘッダーなど）をコンポーネントという部品に分けて管理し、それらをつなぎ合わせて一つのページを構築する
https://beta.reactjs.org/learn/thinking-in-react

# レンダリングとは

1. Triggering a render
2. Rendering the component <-Rendering
3. Committing to the DOM

## Triggering a render

初期レンダリング　 or 　再レンダリングがトリガーになる

```js
//index.jsx 初期レンダリング
const root = createRoot(document.getElementById('root'));
root.render(<Image />);
```

## Rendering the component

レンダリングの差異を調べる
git diff に近い

初期レンダリング=>ルートコンポーネント
再レンダリング=>トリガーした関数コンポーネントを呼び出す

## Committing to the DOM

前回のレンダリングと今回のレンダリングで差異(diff)が生まれたら、その差異の分だけ DOM を変更する（commit）

## git で例えると

```js
// git diff HEAD^
// diff --git a/pages/index.tsx b/pages/index.tsx
// index 86b5b3b..e5d4e5a 100644
// --- a/pages/index.tsx
// +++ b/pages/index.tsx
// @@ -14,7 +14,7 @@
 const Home: NextPage = () => {

       <main className={styles.main}>
         <h1 className={styles.title}>
-          Welcome to <a href="https://nextjs.org">Next.js!</a>  //変更前
+          Welcome to <a href="https://nextjs.org">Next.js!XXXXXXXXXXXXXXXXXXXXXXXXXXXXXKKKKKKKKKK</a>　//変更後
         </h1>

         <p className={styles.description}>
```

git とはバージョン管理システムで、例えば変更を巻き戻したい場合は ver1.2->1.0 に戻すなどして対応できる

- git add 変更を commit の対象にする
- git commit 変更、追加したファイルを git に登録
- git push リモートリポジトリに反映

＊リモートリポジトリとは github 上にあるみんなが編集できるリポジトリ
git add -> git commit した時点で差異があれば git diff を行えば上記のようにその差異が見えるようになる

レンダリングは、コミットできる状態を検知すること？

# この記事を書くに至ったきっかけ

- ハッカソン
  生まれてはじめてハッカソンに参加する機会があり、その際に数カ月ぶりに React ライブラリに触れました。しかし、うまくいくだろうと思っていたルーティングや、React から Nextjs に移行する際にもトラブル続きでうまく行かなかったので、わかるまで原理から学習したいと思いこの記事を書きました。
