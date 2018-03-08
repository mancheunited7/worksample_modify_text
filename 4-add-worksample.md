# Rails入門13

## 今回のテーマ

以下のような、ブログ投稿用の新規画面を作成する

[![https://diveintocode.gyazo.com/f661a7b7cdf1e4e058077ccef3e20a65](https://t.gyazo.com/teams/diveintocode/f661a7b7cdf1e4e058077ccef3e20a65.png)](https://diveintocode.gyazo.com/f661a7b7cdf1e4e058077ccef3e20a65)

## 章の内容
ブログ投稿用の新規画面（`new`）に必要な

- ルーティングを`RESTful`に沿って実装する。
- アクション(`new`)を実装する。
- フォームを`form_with`メソッドを使用して実装する。

## 【RESTful】new

MVCを理解し易いようにRouting、Controller、（Model）、Viewの流れで必要なコードを実装していきます。今回は特にModelは編集しないので、括弧としています。

### Routing

まずは、ブログ新規作成画面のルーティングを作成することから初めましょう。
RESTfulな実装であれば、resourcesメソッドを使用して、ルーティングを設定するのが一般的です。

`config/routes.rb`

```rb
Rails.application.routes.draw do
  # 追記する
  resources :blogs
end
```

`resources`
https://railsguides.jp/routing.html#crud%E3%80%81%E5%8B%95%E8%A9%9E%E3%80%81%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3

#### resources

resourcesメソッドを使用することで、RESTfulに沿った7つのルーティングが生成されます。
実際に`rails routes`コマンドをターミナルで実行して、確かめてみましょう。

```
dive_into_code:~/workspace/sample (master) $ rails routes
   Prefix Verb   URI Pattern               Controller#Action
    blogs GET    /blogs(.:format)          blogs#index
          POST   /blogs(.:format)          blogs#create
 new_blog GET    /blogs/new(.:format)      blogs#new
edit_blog GET    /blogs/:id/edit(.:format) blogs#edit
     blog GET    /blogs/:id(.:format)      blogs#show
          PATCH  /blogs/:id(.:format)      blogs#update
          PUT    /blogs/:id(.:format)      blogs#update
          DELETE /blogs/:id(.:format)      blogs#destroy
```

このように、index/create/new/edit/show/update/destroyへのroutingが生成されているのが分かります。
updateへのroutingが2個生成されているのは、後の仕様でpatchが追加されたためです。特に気にする必要はありません。

```
URL	                    アクション名 HTTPメソッド 説明
/XXXs(.:format)	        index     GET	      一覧画面を生成
/XXXs/:id(.:format)	    show	    GET	      詳細画面を生成
/XXXs/new(.:format)	    new	      GET	      登録画面を生成
/XXXs(.:format)	        create	  POST	    登録処理をする
/XXXs/:id/edit(.:format)edit	    GET	      編集画面を生成
/XXXs/:id(.:format)	    update	  PUT	      更新処理を行う
/XXXs/:id(.:format)	    destroy	  DELETE	  削除処理を行う
```

*（参考）*  http://railsdoc.com/references/resources より

また、prefixとありますが、prefixを利用すると、Viewで利用するlink_to メソッドや、redirect メソッドなどで必要とするパスの指定を簡単に書くことが出来ます。(後のリンク生成で学びます。)

これで、ブログのCRUD機能に必要な、ルーティングを実装することができました。

#### ルーティング確認

先程のルーティングによって、`GET`メソッドで、`/blogs/new`のURLでリクエストが来た場合、Blogsコントローラーのnewアクションを選択するルーティングを作成することができました。

[![https://diveintocode.gyazo.com/7831da463339ea7bd5bc5290da60fe51](https://t.gyazo.com/teams/diveintocode/7831da463339ea7bd5bc5290da60fe51.png)](https://diveintocode.gyazo.com/7831da463339ea7bd5bc5290da60fe51)

### Controller

ルーティングを作成することができたので、アクションをまずは定義しましょう。

`app/controllers/blogs_controller.rb`

```rb
class BlogsController < ApplicationController
  def index
  end

  # 追記する
  def new
  end
end
```


### Viewの作成

続いてViewを編集していきます。まずはviewの作成を行いましょう。
BlogsControllerのnewアクション（メソッド）が呼ばれた場合、Viewはデフォルトで`blogs/new.html.erb`が呼び出されるので、`blogs/new.html.erb`を作成します。

フォルダを右クリックし、NewFileから作成しましょう

### form_with

ファイルを作成できたところで、実際にフォームを作成していきますが、モデルと関連付けたフォームを作成するので、あればform_withメソッドがとても便利です。

例えば、下記のようなform_withメソッドを記述することで、

```
<%= form_with(model: Blog.new) %>
```

以下のようなHTMLを出力することができます。(コードについて詳しく理解する必要はありませんが、HTMLのformタグが生成されていることを確認しましょう)

```html
<form action="/blogs" accept-charset="UTF-8" data-remote="true" method="post">
  <input name="utf8" type="hidden" value="✓">
  <input type="hidden" name="authenticity_token" value="sAc5l6rFDxpzuKpWF73d81UrQrqAmhReoq8xrgOlzz+PgN1U8GKNgsABTWAw3Grj306HdfBoJwx1xkAHMwjdjA==">
</form>
```

`form_with`を使用することで、簡単にフォームを作成できるようになります。

#### ビューヘルパー

form_withはビューヘルパーの一種です。
ビューヘルパーはRailsに用意された、HTMLを生成するメソッドです。
例えば、form_withは、フォームの作成を簡単にします。

例えば、下記ようなHTMLは、

[![https://diveintocode.gyazo.com/29f94c13d79371f31700c02cf95675e5](https://t.gyazo.com/teams/diveintocode/29f94c13d79371f31700c02cf95675e5.png)](https://diveintocode.gyazo.com/29f94c13d79371f31700c02cf95675e5)

```html
<form action="/blogs" accept-charset="UTF-8" data-remote="true" method="post"><input name="utf8" type="hidden" value="✓"><input type="hidden" name="authenticity_token" value="qbu8RFLhyCuAElg1qr7MLYDEdiEINJdEe1JLQFLr7kyWPFiHCEZKszOrvwON33s9CqGz7njGpBasOzrpYkb8/w==">
  <div class="blog_title">
    <label for="blog_title">Title</label>
    <input type="text" name="blog[title]">
  </div>
  <div class="blog_title">
    <label for="blog_content">Content</label>
    <input type="text" name="blog[content]">
  </div>
  <input type="submit" name="commit" value="Create Blog" data-disable-with="Create Blog">
</form>
```

ビューヘルパーを使用することで下記のように記述できます。

```html
<%= form_with(model: Blog.new) do |form| %>
  <div class="blog_title">
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>
  <div class="blog_title">
    <%= form.label :content %>
    <%= form.text_field :content %>
  </div>
  <%= form.submit %>
<% end %>
```

簡潔にそして、簡単に記述することができるようになります。

#### form_with 引数

[少し難しいですが、ドキュメントを見ると分かるように、form_withメソッドにはmodelオプションの引数として、モデルクラスのインスタンスを渡す必要があります。](https://techracho.bpsinc.jp/hachi8833/2017_05_01/39502)

```
<%= form_with(model: モデルのインスタンス) %>
```

form_withはモデルのインスタンスを元に、formを送信すると、どのようなリクエストを送るかを決定します。
つまり、Blogモデルのインスタンスであれば、ブログを作成するようなリクエストを、Userモデルのインスタンスであれば、ユーザーを作成するようなリクエストを作成するということです。（まずはイメージをつかめれば大丈夫です。）

[![https://diveintocode.gyazo.com/c6054cf6b7503342901c91afe5c37da1](https://t.gyazo.com/teams/diveintocode/c6054cf6b7503342901c91afe5c37da1.png)](https://diveintocode.gyazo.com/c6054cf6b7503342901c91afe5c37da1)

### フォームパーツについて

そのフォームに紐づくパーツは以下のようにして生成します。

```html
<%= form_with(model: Blog.new) do |form| %>
  <!-- この中にパーツを記述する -->
<% end %>
```

この`do ~ end(ブロック)`の中で、用意されたメソッド`form.labelやform.submit`を使用することで、フォームに結びついたパーツ（テキストフィールドやSubmitボタン）を作成することが可能になります。

### フォーム作成

実際に app/views/blogs/new.html.erbにform_withメソッドを使用して、フォームを作成しましょう。（ファイルがない場合作成しましょう。）

`app/views/blogs/new.html.erb`

```html
<%= form_with(model: Blog.new, local: true) do |form| %>
  <div class="blog_title">
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>
  <div class="blog_title">
    <%= form.label :content %>
    <%= form.text_field :content %>
  </div>
  <%= form.submit %>
<% end %>
```

`<%= form_with(model: Blog.new, local: true) do |form| %>`form_withメソッドには、Blog.new、つまりブログモデルのインスタンスを引数として渡しています。そうすることで、`<form class="new_blog" id="new_blog" action="/blogs" accept-charset="UTF-8" method="post">`のように、POSTメソッドで`/blogs`URLにリクエストを送信するフォームを作成することができます。

また、form_withメソッドは、デフォルトではJavaScript用のリクエストが発生してしまうので、` local: true`とすることで、HTML用のリクエストを発行するようにします。(難しいので、この時点で理解できなくても問題ありません。)

最後に、formタグとして出力させるので、form_withメソッドは、`<%=`で囲う必要があります。

`フォームパーツについて`

`form.label`はHTMLのlabelタグを作成するメソッドです。
`form.text_field`は、属性(type)をtextに指定した、HTMLのinputタグを作成するメソッドです。
`form.submit`は、属性(type)をsubmitに指定した、HTMLのinputタグを作成するメソッドです。valueは、form_withに渡されたモデルクラスによって代わります。[valueを変更する場合、オプションを指定する必要があります。](http://railsdoc.com/references/submit)

それぞれをdivタグで囲んでいるのは、改行するためです。divタグもしくはbrタグで囲わないと横一列になります。

### サーバーを起動し、確認する

作成したビューを確認しましょう。
ターミナルで、サーバー起動コマンドを実行し、`blogs/new`にアクセスします。

`rails s -b $IP -p $PORT`

[![https://diveintocode.gyazo.com/2c2c54bb0e53568dfc30b969c3c58f68](https://t.gyazo.com/teams/diveintocode/2c2c54bb0e53568dfc30b969c3c58f68.png)](https://diveintocode.gyazo.com/2c2c54bb0e53568dfc30b969c3c58f68)

form_withのリクエスト先は、メソッド上、データベースに保存されていないのであれば、`POST`メソッドで、`/モデル名s/`というURLに送信することが決まっています。
従ってもしRESTfulに沿ったルーティングに設定していない場合（routesで`resources`を使わず、独自のルーティングを設定している場合）、form_withがもつurlオプションやmethodオプションを使用して、適切なリクエストを生成する必要があります。

### まとめ

```
resourcesメソッドを使用してルーティングを設定することで、
素早く必要な部分のルーティング設定をすることができる。
ビューヘルパーの一種であるform_withメソッドを使用することで、
データ送信に必要なHTMLを素早く生成できる。
```

## お疲れ様でした。

質問はこちらのURLにお願いいたします。

https://diveintocode.jp/diver/questions/new

## テキストフィードバックについて

今後のDIVE INTO CODEの発展のために、お時間があれば
フィードバックのご協力をお願い致します。
以下を参考に、DIVERのコメント欄にご投稿して頂けると有難いです。

・テキストの難易度は適切であったか

・テキストの分量は適切であったか

・説明していない用語はなかったか

・テキストに間違いはなかったか

・テキストのゴールに対して適切な内容であったか

・他に説明を加えてほしいことは無かったか

・やってて楽しいテキストであったか

・有益なテキストであったか
