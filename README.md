# railsのメモ
railsに関するメモ、基本の書き方など

* マイグレーション：DBの構造を管理するやつ。

* コントローラー：ユーザーのリクエストを受け取り、モデルやビューヘその情報を渡す橋渡しの役割を持っている。
「http://~」といったリクエストがあると初めに読み込まれるのがコントローラの部分。そのあと、モデルを呼び出しデータを受け取り、ビューに反映する。

* モデル：データベース上にあるテーブルと対応しており、テーブルのデータを簡単に操作する機能を提供する。  
モデルクラスとデータベースファイルの作成には、`rails g model モデル名 カラム名1:データ型1 カラム名2:データ型2`で作成できる。

* データベースを作成するコマンド
`rails db:create` これで、SQlite3の場合、データベース名がファイル名になる。

* ジェネレータコマンド：railsでは開発に必要なファイルを自動で生成してくれるジェネレータコマンドというのがある。
コントローラを作成する場合には、
`rails generate コントローラ名　アクション名`＊generateはgで省略可能とかく。他にもジェネレータコマンドはたくさんある。

* アクション：コントローラのメソッドのこと。
* ルーティング：URLにアクセスしたとき、どのコントローラのどのアクションに処理を振り分けるかを定義する仕組み。ルーティング情報はconfig/routes.rbに記述する決まり。  
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


## データベース関連
* RDB(Relational DateBase)：データベースの種類のことで、データをテーブルと呼ばれる表形式で表、データの操作にはSQLを使用する。
* コネクションプーリング：データベースへの接続をアクセスのたびに１から行うのではなく、データベース接続（コネクション）をアプリ側で保持して使い回す仕組み。
* O/R(Object/Relational)マッピング：オブジェクト指向プログラミング言語からRDBにアクセスする際の架け橋となる仕組み。
* O/Rマッパー：O/Rマッピングの考え方に基づいて実装されたライブラリのこと。
* ActiveRecord：railsが採用するORマッパー。SQLを書かなくてもデータの登録などが可能といった機能がある。railsではテーブルごとに作成されたモデルクラスを通じてデータベースに接続する。
* 

## 開発中のちょっとしたこと（問題と解決方法とか）
* sass使うなら、`gem 'sassc'`をgemfileに入れて、`bundle install`しないとダメっぽい。あと、cssのコメントのところをsassに移植する。
* link_toとかformとかrubyコード埋め込んでる時のclassの付け方:　`form.submit, class: "任意のクラス名"`
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
原因は、`data: {confirm: '削除してよろしいですか？' }`だとJSが反応していないこと。

* コード直してもうまくいかない時は、一回pumaを再起動する。


