# railsのメモ
railsに関するメモ、基本の書き方など

## 用語
* マイグレーション：DBの構造を管理するやつ。
* アクション：コントローラ内で定義されるメソッドのこと。

* コントローラー：ユーザーのリクエストを受け取り、モデルやビューヘその情報を渡す橋渡しの役割を持っている。
「http://~」といったリクエストがあると初めに読み込まれるのがコントローラの部分。そのあと、モデルを呼び出しデータを受け取り、ビューに反映する。

* モデル：データモデルとして扱うデフォルトのデータ構造のことで、データベース上にあるテーブルと対応しており、テーブルのデータを簡単に操作する機能を提供する。
モデルクラスとデータベースファイルの作成には、`rails g model モデル名 カラム名1:データ型1 カラム名2:データ型2`で作成できる。

* アクティブレコード（Active Record）：データベースとやりとりをするデフォルトのRailsライブラリはActive Recordと呼ばれ、データオブジェクトの作成/保存/検索のためのメソッドを持っている。

* データベースを作成するコマンド
`rails db:create` これで、SQlite3の場合、データベース名がファイル名になる。

* ジェネレータコマンド：railsでは開発に必要なファイルを自動で生成してくれるジェネレータコマンドというのがある。
* コントローラを作成する場合には、`rails generate コントローラ名　アクション名1 アクション名2`＊generateはgで省略可能。また、アクション名は複数記載可能。
* コントローラの削除：`rails destroy コントローラ名　アクション名1 アクション名2`

* アクション：コントローラのメソッドのこと。

* ルーティング：URLにアクセスしたとき、どのコントローラのどのアクションに処理を振り分けるかを定義する仕組み。ルーティング情報はconfig/routes.rbに記述する決まり。
  （ルーターはコントローラとブラウザの間に配置され、ブラウザからのリクエストをコントローラに振り分ける（=ルーティング）役割を果たす。）
```config/routes.rb
get ‘/任意のurl’, to: ‘対応させたいコントローラ名#アクション名’
```  
```config/routes.rb
例　get '/users', to: 'users#index'
```
* getメソッドでは、下記のように、to以下を省略可能。また、アクションですべき処理がない場合はコントローラのアクションも省略可能。
```config/routes.rb
get '/users/new'
```

* scaffolding：scaffoldとは「足場」の意味。このコマンドを用いると、データを参照・登録・更新・削除するための一連の画面が生成される。  
`rails g scaffold リソース名 カラム名1:データ型1 カラム名2:データ型2`  
＊リソース名：モデルクラスの名前

* HTTPリクエストメソッド（orリクエストメソッド）：ブラウザからの要求（リクエスト）がどんな処理を求めているかを区別するための取り決め。GET、POST ,PUT ,PATCH,DELETEなどがある。大抵、一般的なアクセスはGET、フォームからのデータ送信はPOST。

* コントローラでlocalhost:3000にアクセスした時、indexページを表示する処理。  
usersディレクトリ直下のindex.html.erbを表示する場合
```users_controller.rb
def index
  render template: 'users/index'
end
```
＊この時、呼び出すビューファイルが、app/views/コントローラ名/ビュー名.html.erbという形式の場合、renderメソッドは省略しても良い。

* render：コントローラに備わっているメソッドで、クライアントに返すレスポンスを定義する。
* plain:　はその後の値で指定された文字列をクライアントに返すための指定。
* インスタンス変数：クラスのオブジェクト（インスタンス）ごとに保持される変数のこと。railsではリクエストごとにコントローラクラスのインスタンスが生成され、コントローラのインスタンス変数をビューと共有して、コントローラからビューへデータを受け渡している。
* ERBの記述ルール  
`<% %>` rubyのコードとして実行.  
`<%= %>` rubyコードの実行結果の戻り値を出力
* layouts：ページ共通のデザインのテンプレート置き場。railsではビューを生成するタイミングで、まずapplication.html.erbを解釈する。
* link_toヘルパーメソッド
```index.html.rb
<%= link_to リンク名,リンク先のパス %>
<%= link_to "新規投稿", new_diary_path %>
```
link_toで指定するリンク先には、ルーティングの出力結果に表示されていたPrefixに_pathをつけた形式を指定することができる。ルーティングで出力されるPrefixは、画面パスへの省略形の_pathを除いたもの。

* Prefix：ルーティングやパスヘルパーにおいて、コントローラーやアクションの名前の先頭に付加される識別子です。具体的には、各ルートには名前が付けられ、その名前には通常、コントローラー名やアクション名のプレフィックスが付けられる。これにより、名前の衝突を防ぎ、コードの可読性を高めることができる。例えば、resources :articlesというルートがある場合、このルートにはいくつかのパスヘルパーが自動的に生成されます。これらのパスヘルパーの名前は、通常、アクション名に対応しますが、先頭にコントローラー名が付加されます。
  - articles_path: 記事の一覧を表示するためのパス
  - new_article_path: 新しい記事を作成するためのパス  
これにより、各パスヘルパーの名前が一意になり、コードがより読みやすくなります。

* before_actionメソッド：Railsのコントローラー内で使用されるフィルターメソッドの1つ。これは、コントローラー内のアクションが実行される前に、事前に指定されたメソッド（またはブロック）を実行するために使用される。
```controller.rb
before_action メソッド名, 条件ハッシュ
```
メソッド名は、コントローラないのメソッドをシンボルで記述する。条件ハッシュは、適用するアクションの条件をハッシュで記述する。



## その他

### `rails routes`コマンド
routes.rbの定義がどうなっているかを確認するコマンド

### クロスサイトスクリプティング攻撃とは
　クロスサイトスクリプティング（Cross-Site Scripting、XSS）攻撃は、Webアプリケーションにおいてよく見られるセキュリティ上の脆弱性の一つ。この攻撃では、攻撃者がWebページに不正なスクリプト（通常はJavaScript）を埋め込んで、ユーザーのブラウザで実行させることが目的。XSS攻撃は、主に次の2つのタイプに分類される：
1. Stored XSS (永続的XSS): 攻撃者がWebアプリケーションのデータベースやファイルに不正なスクリプトを格納することに成功した場合に発生する。その後、ユーザーがそのデータを閲覧したり表示したりすると、不正なスクリプトが実行される。例えば、掲示板やコメント欄に不正なスクリプトを投稿することで、多くのユーザーに影響を与えることになる。
2. Reflected XSS (反射型XSS): 攻撃者がユーザーに偽のリンクを送り、ユーザーがそのリンクをクリックすると、不正なスクリプトが含まれたURLがWebアプリケーションに送信される。WebアプリケーションはそのURLを解析し、不正なスクリプトを含むレスポンスを返す。ユーザーのブラウザがそのレスポンスを表示すると、不正なスクリプトが実行される。  

　XSS攻撃の結果としては、以下のようなことが起こり得る：
- ユーザーのセッションクッキーや認証トークンが盗まれる。
- ユーザーが入力した個人情報や機密情報が盗まれる。
- ユーザーが意図しない操作を行わされる（例：ログアウトさせられる、偽のフォームを送信させられるなど）。  

　XSS攻撃を防ぐためには、適切な入力検証、エスケープ処理、HTTPヘッダーの設定、Content Security Policy（CSP）の実装など、複数のセキュリティ対策が必要となる。railsにおける`<%= csp_meta_tag %>`はその対策となるコード。

### クロスサイトリクエストフォージェリー攻撃とは
　クロスサイトリクエストフォージェリー（Cross-Site Request Forgery、CSRF）攻撃は、悪意のある攻撃者がユーザーのブラウザを操作して、そのユーザーが権限を持つWebアプリケーション上で不正なリクエストを送信させる攻撃手法。攻撃者は、ユーザーが訪れる悪意のあるWebサイトや、メール、掲示板などを通じて不正なリクエストを送信するリンクやフォームを提供する。このリンクやフォームは、攻撃者が事前に作成した悪意のあるリクエストを実行するためのもの。ユーザーが悪意のあるリンクをクリックしたり、不正なフォームを送信したりすると、そのユーザーのブラウザは攻撃者が用意した不正なリクエストを自動的に送信する。このリクエストは、ユーザーがログインしているWebアプリケーションに対して送信されるため、アプリケーションはそれを正当なものと見なし、実行する。
　CSRF攻撃の結果としては、以下のようなことが起こり得ます：
- ユーザーのアカウントの権限を悪用して、意図しない操作を実行する（例：他のユーザーのアカウントに勝手にフォローする、メールアドレスを変更するなど）。
- 機密情報が漏洩する（例：メールアドレスやパスワードが変更される）。
- 攻撃者が操作を偽装して、ユーザーに誤解を与える（例：偽の支払いを行うなど）。  

　CSRF攻撃を防ぐためには、CSRFトークンを使用するなどの対策が必要。CSRFトークンは、セッションごとに生成され、フォームやリンクに含まれることで、リクエストの正当性を検証するのに使用される。また、SameSite属性やリファラーヘッダーの制限、HTTPSの使用なども有効な対策となる。railsにおける`<%= csrf_meta_tags %>`はその対策となるコード。

## データベース関連
* RDB(Relational DateBase)：データベースの種類のことで、データをテーブルと呼ばれる表形式で表、データの操作にはSQLを使用する。
* コネクションプーリング：データベースへの接続をアクセスのたびに１から行うのではなく、データベース接続（コネクション）をアプリ側で保持して使い回す仕組み。
* O/R(Object/Relational)マッピング：オブジェクト指向プログラミング言語からRDBにアクセスする際の架け橋となる仕組み。
* O/Rマッパー：O/Rマッピングの考え方に基づいて実装されたライブラリのこと。
* ActiveRecord：railsが採用するORマッパー。SQLを書かなくてもデータの登録などが可能といった機能がある。railsではテーブルごとに作成されたモデルクラスを通じてデータベースに接続する。


## 開発中のちょっとしたこと（問題と解決方法とか）
* sass使うなら、`gem 'sassc'`をgemfileに入れて、`bundle install`しないとダメっぽい。あと、cssのコメントのところをsassに移植する。＊ちょっとSassに関しては色々調べないとダメだ。
* link_toとかformとかrubyコード埋め込んでる時のclassの付け方:　`form.submit, class: "任意のクラス名"`
* 命名：コントローラ名には複数形を使い、モデル名には単数形を用いる。
### ・投稿時のバリデーションに引っかかった際、エラーメッセージが出ない
- 解決方法1：~~import Rails from "./rails-ujs”;をjavascript/application.jsに書くとエラーメッセージが出るようになった~~ これはだめだった。
- 解決方法2:
- エラーメッセージ`Error: Form responses must redirect to another location`と出る。
- 原因は、createメソッドのelseの処理で、リクエストが成功し要求された処理が正常に完了したことを示すHTTPステータスコード200を返していることが問題となっていると思われる。
- そこで、createメソッドのelseの処理に、`status: :unprocessable_entity`を追記し、処理ができなかったことを意味するHTTPステータスコード422を返すようにする。そうしたらエラーが消えた。
- なお、必須のフィールドが欠落したいたりするなどで、フォームの送信に失敗した場合、HTTPステータスコード422 Unprocessable Entityを返すことが一般的らしい。

### ・エラーメッセージの内容を変えたい(nameを入力してくださいになってしまう)
解決方法：モデルの翻訳情報を追加すればいい（config/locales/ja.yml）（速習実践ガイド　p103を参照）

### ・削除した時に確認メッセージ（confirmが機能しない）が出ない
解決方法  
 ```controller.rb
 = button_to "削除", post, method: :delete, data: {confirm: '削除してよろしいですか？' }
```
上記のコードを次のように変える。
```controller.rb
= button_to "削除", post, method: :delete, data: {turbo_confirm: '削除してよろしいですか？' }
```
原因は、`data: {confirm: '削除してよろしいですか？' }`だとJSが反応していないこと。理由はよく分からない。なお、`button_to`が`link_to`だと機能しなくなる。

### Rspecが機能しない
`gem "rspec-rails"`と`gem "factory_bot_rails"`のバージョンの指定を消したら動いた。どうやら指定していたバージョンが古かったせいだと思われる。
  

### 原因が不明なエラーの際
* コード直してもうまくいかない時は、一回pumaを再起動する。
* gemのバージョンが古い可能性。バージョンの指定（~>5.1など）を消すと治る場合がある。

### dupメソッド
dupは、同じ属性を持つデータを複製するためのメソッド。例えば、testなどで用いる。
```test.rb
test "email addresses should be unique" do
    duplicate_user = @user.dup
    @user.save
    assert_not duplicate_user.valid?
  end
```
### authenticateメソッド
このメソッドは、引数に渡された文字列（パスワード）をハッシュ化した値と、データベース内にあるpassword_digestカラムの値を比較する。
 
### `resources :users`で可能となるパス
* HTTPリクエストメソッド	URL	アクション	名前付きルーティング	用途
* GET	/users	index	users_path	すべてのユーザーを一覧するページ
* GET	/users/1	show	user_path(user)	特定のユーザーを表示するページ
* GET	/users/new	new	new_user_path	ユーザーを新規作成するページ（ユーザー登録）
* POST	/users	create	users_path	ユーザーを作成するアクション
* GET	/users/1/edit	edit	edit_user_path(user)	id=1のユーザーを編集するページ
* PATCH	/users/1	update	user_path(user)	ユーザーを更新するアクション
* DELETE	/users/1	destroy	user_path(user)	ユーザーを削除するアクション
