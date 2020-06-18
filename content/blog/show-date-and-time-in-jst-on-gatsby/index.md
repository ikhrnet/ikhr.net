---
title: Gatsbyで日本時間の日時を表示する
date: "2020-06-19T02:30:05.887+09:00"
description: GatsbyのタイムゾーンはUTCになっている。moment.jsを使いJSTに変換する。
---

このサイトは[Gatsby](https://www.gatsbyjs.org)で構築しています。日時がUTCになっていることに先日気づきました。管理画面のあるCMSならタイムゾーンの変更は一瞬ですし、[Jekyll](https://jekyllrb.com/docs/configuration/options/)でも設定項目があります。

Gatsbyの場合、一箇所で設定するような仕組みがないのか情報が少なく、しばらく詰まりました。幸い日本の方が[Issue](https://github.com/gatsbyjs/gatsby/issues/11832#issuecomment-464797581)を上げてくれていて、変更方法がわかりました。

[gatsby-starter-blog](https://github.com/gatsbyjs/gatsby-starter-blog)では、ホームのテンプレートとなる[index.js](https://github.com/gatsbyjs/gatsby-starter-blog/blob/d0f6ce5487d6dee3b843fd568264713c84acfb89/src/pages/index.js#L63-L67)と、個別記事の[blog-post.js](https://github.com/gatsbyjs/gatsby-starter-blog/blob/d0f6ce5487d6dee3b843fd568264713c84acfb89/src/templates/blog-post.js#L94-L98)でほぼ同じ記述がGraphQL内に見つかります。

```graphql
frontmatter {
  title
  date(formatString: "MMMM DD, YYYY")
  description
}
```

Markdownで書かれた記事のフロントマターから`date`の値を取ってきて変換しています。この部分でタイムゾーンを指定するオプションを追加してよというのがIssueの内容でした。

それに対して、クエリの中でなくページの方で変換すればいいんでないのという回答でした。クエリの方では`date`を加工せずに単に返せばいいということで、先程のスニペットは以下のようになります。

```graphql
frontmatter {
  title
  date
  description
}
```

Gatsbyは[moment.js](https://momentjs.com)というパッケージを[使用している](https://github.com/gatsbyjs/gatsby-starter-blog/blob/d0f6ce5487d6dee3b843fd568264713c84acfb89/package-lock.json#L9071)ようです。ですので各ファイルの先頭付近でmoment.jsを読み込みます。

```javascript
import moment from "moment"
```

`date` が使用されている箇所を[`index.js`](https://github.com/gatsbyjs/gatsby-starter-blog/blob/d0f6ce5487d6dee3b843fd568264713c84acfb89/src/pages/index.js#L31)や[`blog-post.js`](https://github.com/gatsbyjs/gatsby-starter-blog/blob/d0f6ce5487d6dee3b843fd568264713c84acfb89/src/templates/blog-post.js#L37)で見つけます。

```tsx
{post.frontmatter.date}
```

これを次のように変更すれば出来上がりです。

```javascript
{moment(post.frontmatter.date).format(`YYYY-MM-DD HH:mm`)}
```
