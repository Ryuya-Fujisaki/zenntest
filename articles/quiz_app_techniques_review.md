---
title: "ひらがな学習ゲームアプリ使用技術振り返り"
emoji: "👻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Firebase Authentication", "Clerk"]
published: true
# published_at: 2025-05-04 21:00 # 未来の日時を指定する
---
ある企業の技術面接で、**Firebase Authentication**の仕組みを理解しているかと問われ、答えられなかったので、ここで改めて備忘録としてメモしておく。

[ひらがな学習ゲームアプリ](https://hiragana-learning.vercel.app/)では、Firebase Authenticationによりユーザー認証機能を実装しており、Googleアカウントによる認証を用いている。

Firebase Authentication SDKのライブラリであるFirebaseUI Authを利用して実装しており、非常に簡単。ドロップイン認証ソリューションという、必要な設定項目を記述するだけでUIから認証処理まで全て埋め込んでくれている。

Firebase Authenticationは**ステートレスのトークン認証**を採用しており、どのユーザーがログイン中なのかという状態をFirebaseバックエンド側で管理していない。このステートレスというのがFirebase Authenticationの良いところで、**ステートフル＝セッション認証**だと、サーバーがセッション情報を保持し毎回クッキーによって識別するんだけど、そうなるとサーバー間でセッション共有が必要で、セッションの管理が大変らしい。それに対してステートレス＝トークン認証だと、毎回リクエスト時にヘッダーに含まれているトークンを検証するだけでユーザーが誰かがわかる。どのサーバーでもトークンを検証するだけでユーザーの特定が可能で、セッションを共有する必要もない。マイクロサービス間での連携が容易で**スケーラブル**である。なるほど。

認証成功後、Firebase Authenticationはトークンを発行し、それをブラウザーのIndexedDBに保存している。トークンはFirebase ID Tokenで、**JWT(JSON Web Token)**仕様。トークンをデコードすると、Uidや認証プロバイダー情報が含まれており、有効期間は1時間。

トークンが保存される**IndexedDB**は**LocalStorage**の上位版で、LocalStorageはKey=Valueペアのような文字列しか保存できないのに対し、IndexedDBでは**JSONオブジェクト**が保存できる。

尚、[クイズアプリ](https://my-app-theta-two-42.vercel.app/)で実装した**Clerk**による認証もJWT形式のトークンを用いているが、高品質なUIが提供される点、カスタマイズ性が高い、Next.jsなどモダンFWと相性が良い点などはFirebase AuthenticationよりもClerkの方が優れており、WebアプリでモダンなUXを実現したい場合はClerkがおすすめ。モバイルアプリやGCP全体と統合したい場合はFirebaseが強みを発揮すると考えられる。

