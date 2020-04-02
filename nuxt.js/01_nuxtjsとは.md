# nuxt.jsとは

## 特徴

公式サイト引用

> *Nuxtは、モダンな web アプリケーションを作成する Vue.js に基づいたプログレッシブフレームワークです。Vue.js 公式ライブラリ（vue、vue-router や vuex）および強力な開発ツール（webpack、Babel や PostCSS）に基づいています。 Nuxt の目標は、優れた開発者エクスペリエンスを念頭に置き、Web 開発を強力かつ高性能にすることです。*



vueを用いてフロントエンドのアプリケーションが作成が可能なフレームワークであるが、vue単体で開発するのと違って最大の特徴は**SSR(サーバサイドレンダリング)**に対応しているところである。

SSRするメリットとデメリットを説明するにはSPA（シングルページアプリケーション）のメリット・デメリットを比較するとわかりやすい。

- SPA
  - メリット
    - 実装しやすい
    - サーバの準備が楽
      - S3などにあげればサーバは不要
      - GitHub Pagesへのデプロイ可能
  - デメリット
    - 初期表示が遅い
    - SEOに不安がある
      - 表示のレンダリングが行われる((javascriptが実行される)前のhtmlで検索エンジンに収集されてしまう可能性がある
      - Gooleはjavascript実行後のページで収集してくれるかも？？[Official Google Webmaster Central Blog: Understanding web pages better](https://webmasters.googleblog.com/2014/05/understanding-web-pages-better.html)
    - OGをページごとに設定できない
      - **OGってなに？？**
- SSR
  - メリット
    - SPAの欠点を解消できる
      - サーバサイドレンダリングしてからクライアントに渡されるため初期表示が早い
        - サーバが非力だと結局遅い？
      - レンダリングしたhtmlが検索エンジンに収集されるのでSPAと比べてSEO対策ができる
  - デメリット
    - 実装コストが増える
      - Next.jsの使い勝手はまだ不明だが、少なくともVue + αの知識が必須となる
    - サーバの構築が必要
      - フロントエンドのエンジニアだけで構築するのは困難かも



また設定を変えることで**静的ファイルへのビルド**も可能。



他の特徴としてNuxt.jsには最初から以下のライブラリが用意されている

- Vue Router
- Vuex
- Vue Server Renderer
- Vue Meta
- axios



学習コストはかかるが、実装経験があるチームであればアプリケーション構造も統一化できるのでSPAアプリ開発でもNuxt.jsを使用していても問題ないと思われる。



## 参考サイト

- [はじめに - NuxtJS](https://ja.nuxtjs.org/guide/#%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%B5%E3%82%A4%E3%83%89%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0)
  - 公式サイト
- [Nuxt.jsとは？Nuxt.jsを採用する前に知っておきたいこと｜たのしいWeb開発](https://dev83.com/nuxtjs-about/)
- [NuxtとVueの違いは？実装して感じたNuxt.jsのメリット | MK Dev](https://mk-engineer.com/posts/nuxt-start/)

