## 設計

https://cloud.google.com/apis/design?hl=ja

https://qiita.com/KNR109/items/d3b6aa8803c62238d990

https://developer.ntt.com/ja/blog/522676d0-7336-4139-b70a-ec6059e10cf3

----

## APIモードとは

RailsのAPIモードは、Railsアプリケーションの中でも、APIサーバーを開発するために特化したモードです。

APIモードを使用することで、Railsアプリケーションに必要のない機能やファイルを最小限に抑えることができ、APIサーバーの開発に集中することができます。また、APIモードでは、アプリケーションのスタックに不要な要素を含まず、必要最小限のパッケージがインストールされるため、アプリケーションの起動時間やレスポンス時間を短縮することができます。

APIモードでは、デフォルトの設定で、RailsのView機能やAsset Pipeline機能が無効になっています。また、APIモードでは、ActiveRecordのテンプレートファイルやアセットファイルを含む生成物を除去することもできます。

APIモードでは、APIサーバー開発に必要なライブラリやGemがデフォルトでインストールされています。例えば、Active Model SerializersやGrapeなど、API開発に必要なライブラリが含まれています。また、Rails 5.0からは、APIモードの標準として、API-onlyアプリケーションの開発に必要なAction ControllerのAPIモジュールが提供されています。

以上のように、APIモードは、APIサーバーの開発に必要な要素を最小限に抑えた簡素なアプリケーションを構築することができます。

----

RailsでAPIを作成する際に気をつける点は、以下のようなものがあります。

1. セキュリティ
APIには、機密情報が含まれることがあります。したがって、セキュリティを考慮して、APIのエンドポイントやアクセス許可を適切に設定する必要があります。また、パスワードや認証トークンなどの機密情報を平文で送信しないようにする必要があります。

2. パフォーマンス
APIは、多くのリクエストを処理することができるように設計する必要があります。したがって、パフォーマンスの最適化を行うことが重要です。例えば、キャッシュやプリロードなどの技術を使用して、レスポンス時間を短縮することができます。

3. ドキュメンテーション
APIを使用する開発者やクライアントに対して、APIのドキュメンテーションを提供することが重要です。ドキュメンテーションには、APIのエンドポイント、パラメータ、レスポンスの形式などが含まれます。

4. テスト
APIには、多くのリクエストを処理するため、多数のバグが発生する可能性があります。したがって、APIのテストを適切に実施することが必要です。テストには、APIの各エンドポイントをカバーする単体テストや、APIのエンドポイント間の相互作用をテストする統合テストが含まれます。

5. バージョン管理
APIは、長期間使用されることがあります。したがって、APIの変更やアップデートに対応するために、バージョン管理を行うことが必要です。APIのバージョン管理には、URLパラメーターやHTTPヘッダーなどの方法があります。

以上のように、APIを開発する際には、セキュリティ、パフォーマンス、ドキュメンテーション、テスト、バージョン管理など、様々な要素に注意を払う必要があります。

```ruby
# config/routes.rb
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :users, only: [:index, :show, :create, :update, :destroy]
      # APIのバージョン管理を行うため、namespaceを使用しています。
      # resourcesでAPIのエンドポイントを定義しています。
    end
  end
end
```

```ruby
# app/controllers/api/v1/users_controller.rb
module Api
  module V1
    class UsersController < ApplicationController
      before_action :set_user, only: [:show, :update, :destroy]

      # GET /users
      def index
        @users = User.all
        render json: @users
      end

      # GET /users/1
      def show
        render json: @user
      end

      # POST /users
      def create
        @user = User.new(user_params)

        if @user.save
          render json: @user, status: :created, location: api_v1_user_url(@user)
        else
          render json: @user.errors, status: :unprocessable_entity
        end
      end

      # PATCH/PUT /users/1
      def update
        if @user.update(user_params)
          render json: @user
        else
          render json: @user.errors, status: :unprocessable_entity
        end
      end

      # DELETE /users/1
      def destroy
        @user.destroy
      end

      private
        # Use callbacks to share common setup or constraints between actions.
        def set_user
          @user = User.find(params[:id])
        end

        # Only allow a trusted parameter "white list" through.
        def user_params
          params.require(:user).permit(:name, :email)
        end
    end
  end
end
```
上記のコードでは、APIのエンドポイントを定義し、各エンドポイントに対応するアクションを実装しています。また、セキュリティに対応するため、strong parameterを使用して、入力された値の検証を行っています。さらに、バージョン管理を行うために、namespaceを使用して、APIのバージョンを管理しています。

上記のコードではパフォーマンスについては直接的には考慮されていませんが、以下のような対応が考えられます。

クエリの最適化: indexアクションでUser.allというクエリを発行しているため、データ量が大きくなるとパフォーマンスに影響が出る可能性があります。このため、必要最低限のデータのみを取得するようなクエリを発行するなどの最適化が必要です。
キャッシュの導入: クエリの最適化以外にも、よくアクセスされるデータをキャッシュすることで、リクエストに対するレスポンス時間を短縮することができます。
バッチ処理の導入: 大量のリクエストが集中した場合に、負荷分散のためにバッチ処理を行うなどの対応を行うことができます。
上記のような対応を行うことで、REST APIのパフォーマンスを向上させることができます。

----

以下は、上記のコードに対するRSpecによるテストの例です。

```ruby
# spec/requests/api/v1/users_spec.rb
require 'rails_helper'

RSpec.describe "Api::V1::Users", type: :request do
  describe "GET /api/v1/users" do
    it "returns http success" do
      get "/api/v1/users"
      expect(response).to have_http_status(:success)
    end
  end

  describe "GET /api/v1/users/:id" do
    let!(:user) { create(:user) }

    it "returns http success" do
      get "/api/v1/users/#{user.id}"
      expect(response).to have_http_status(:success)
    end
  end

  describe "POST /api/v1/users" do
    let(:valid_attributes) { { user: { name: "Test User", email: "test@example.com" } } }
    let(:invalid_attributes) { { user: { name: "", email: "" } } }

    context "with valid attributes" do
      it "creates a new user" do
        expect {
          post "/api/v1/users", params: valid_attributes
        }.to change(User, :count).by(1)
      end

      it "returns http created" do
        post "/api/v1/users", params: valid_attributes
        expect(response).to have_http_status(:created)
      end
    end

    context "with invalid attributes" do
      it "does not create a new user" do
        expect {
          post "/api/v1/users", params: invalid_attributes
        }.to_not change(User, :count)
      end

      it "returns http unprocessable_entity" do
        post "/api/v1/users", params: invalid_attributes
        expect(response).to have_http_status(:unprocessable_entity)
      end
    end
  end

  describe "PUT /api/v1/users/:id" do
    let!(:user) { create(:user) }
    let(:valid_attributes) { { user: { name: "Updated User Name" } } }
    let(:invalid_attributes) { { user: { name: "" } } }

    context "with valid attributes" do
      it "updates the user" do
        put "/api/v1/users/#{user.id}", params: valid_attributes
        user.reload
        expect(user.name).to eq("Updated User Name")
      end

      it "returns http success" do
        put "/api/v1/users/#{user.id}", params: valid_attributes
        expect(response).to have_http_status(:success)
      end
    end

    context "with invalid attributes" do
      it "does not update the user" do
        put "/api/v1/users/#{user.id}", params: invalid_attributes
        user.reload
        expect(user.name).to_not eq("")
      end

      it "returns http unprocessable_entity" do
        put "/api/v1/users/#{user.id}", params: invalid_attributes
        expect(response).to have_http_status(:unprocessable_entity)
      end
    end
  end

  describe "DELETE /api/v1/users/:id" do
    let!(:user) { create(:user) }

    it "destroys the user" do
      expect {
        delete "/api/v1/users/#{user.id}"
      }.to change(User, :count).by(-1)
    end

    it "returns http success" do
      delete "/api/v1/users/#{user.id}"
      expect(response).to have
      expect(response).to have_http_status(:success)
    end
  end
end
```

このテストコードでは、それぞれのエンドポイントがHTTPリクエストに対して正しいHTTPレスポンスを返すかどうかを検証しています。また、リクエストパラメータの検証も行っており、正しくバリデーションされていることを確認しています。このように、APIを作成する際には、適切なテストを行い、正しく動作することを確認することが非常に重要です。

----

OpenAPI（Swagger）でのドキュメント作成には、以下の手順を踏みます。

1. swagger.ymlファイルの作成
2. エンドポイントの定義
3. パラメーターの定義
4. レスポンスの定義
以下は、上記のAPIをOpenAPIでドキュメント化した例です。

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
paths:
  /api/v1/users:
    get:
      summary: Get all users
      responses:
        200:
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: Create a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        201:
          description: Created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
  /api/v1/users/{id}:
    get:
      summary: Get a user
      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true
          description: User ID
      responses:
        200:
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
    put:
      summary: Update a user
      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true
          description: User ID
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        200:
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
    delete:
      summary: Delete a user
      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true
          description: User ID
      responses:
        204:
          description: No Content
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
          format: email
      required:
        - name
        - email
```

このようにOpenAPIでドキュメントを作成することで、APIの仕様やエンドポイントのURL、パラメーター、レスポンスなどを明確にし、開発者がAPIを簡単に理解できるようにすることができます。

----

以下は、上記のREST APIのコードをGraphQL APIに置き換えた例です。

```ruby
class UserApiSchema < GraphQL::Schema
  mutation(Types::MutationType)
  query(Types::QueryType)
end

module Types
  class QueryType < Types::BaseObject
    field :users, [UserType], null: false

    def users
      User.all
    end
  end

  class MutationType < Types::BaseObject
    field :create_user, UserType, null: false do
      argument :name, String, required: true
      argument :email, String, required: true
    end

    field :update_user, UserType, null: false do
      argument :id, ID, required: true
      argument :name, String, required: true
      argument :email, String, required: true
    end

    field :delete_user, Boolean, null: false do
      argument :id, ID, required: true
    end

    def create_user(name:, email:)
      User.create(name: name, email: email)
    end

    def update_user(id:, name:, email:)
      user = User.find(id)
      user.update(name: name, email: email)
      user
    end

    def delete_user(id:)
      user = User.find(id)
      user.destroy
      true
    end
  end

  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :name, String, null: false
    field :email, String, null: false
  end
end
```

GraphQL APIでは、REST APIとは異なり、エンドポイントに対して一つのURLを割り当て、クライアント側から必要なデータをリクエストする形式で通信します。また、クライアント側からリクエストされたデータのみを返すため、不必要な情報のやり取りが発生しません。これにより、通信の負荷が軽減され、APIのパフォーマンスが向上することが期待できます。

----

GraphQL APIの場合、通信の形式がREST APIと異なるため、テストの書き方も異なります。REST APIの場合、エンドポイントとHTTPメソッドでリクエスト内容を指定するのに対し、GraphQL APIではGraphQLのクエリをリクエストの本文に含める必要があります。

以下はGraphQL APIに対するRSpecのテストの例です。

```ruby
RSpec.describe UserApiSchema do
  describe "users query" do
    let!(:users) { create_list(:user, 3) }
    let(:query) do
      <<~QUERY
        query {
          users {
            id
            name
            email
          }
        }
      QUERY
    end

    it "returns users" do
      post "/graphql", params: { query: query }
      expect(response).to have_http_status(:success)
      json_response = JSON.parse(response.body)
      expect(json_response["data"]["users"].size).to eq(users.size)
    end
  end

  describe "createUser mutation" do
    let(:mutation) do
      <<~MUTATION
        mutation($name: String!, $email: String!) {
          createUser(input: {name: $name, email: $email}) {
            user {
              id
              name
              email
            }
          }
        }
      MUTATION
    end

    it "creates a user" do
      post "/graphql", params: { query: mutation, variables: { name: "Alice", email: "alice@example.com" } }
      expect(response).to have_http_status(:success)
      json_response = JSON.parse(response.body)
      expect(json_response["data"]["createUser"]["user"]["name"]).to eq("Alice")
      expect(json_response["data"]["createUser"]["user"]["email"]).to eq("alice@example.com")
    end
  end

  describe "updateUser mutation" do
    let!(:user) { create(:user) }
    let(:mutation) do
      <<~MUTATION
        mutation($id: ID!, $name: String!, $email: String!) {
          updateUser(input: {id: $id, name: $name, email: $email}) {
            user {
              id
              name
              email
            }
          }
        }
      MUTATION
    end

    it "updates a user" do
      post "/graphql", params: { query: mutation, variables: { id: user.id, name: "Bob", email: "bob@example.com" } }
      expect(response).to have_http_status(:success)
      json_response = JSON.parse(response.body)
      expect(json_response["data"]["updateUser"]["user"]["name"]).to eq("Bob")
      expect(json_response["data"]["updateUser"]["user"]["email"]).to eq("bob@example.com")
    end
  end

  describe "deleteUser mutation" do
    let!(:user) { create(:user) }
    let(:mutation) do
      <<~MUTATION
        mutation($id: ID!) {
          deleteUser(input: {id: $id})
        }
      MUTATION
    end

    it "deletes a user" do
      post "/graphql", params: { query: mutation, variables: { id: user.id } }
      expect(response).to have_http_status(:success)
      json_response = JSON.parse(response.body)
      expect(json_response["data"]["deleteUser"]).to eq(true)
      expect(User.find_by(id: user.id)).to be_nil
    end
  end
end
```

この例では、GraphQLのmutationをリクエストしています。また、リクエストのパラメータはvariablesに渡しています。そのため、requestテストの書き方も変わります。リクエストのレスポンスをパースしてJSONオブジェクトとして取得し、それを元にテストを行います。また、削除されたかどうかを確認するために、削除されたユーザーがDBから取得できないことを確認しています。

----

GraphQLの場合、OpenAPIでのドキュメントの書き方は異なります。GraphQL用のAPIドキュメントは、OpenAPIではなく、GraphQLのスキーマ言語であるSDL（Schema Definition Language）を使用して記述することが一般的です。

例えば、GraphQLの公式ライブラリであるApolloには、SDLを使用してGraphQLのスキーマを記述することができるApollo Serverというものがあります。この場合、スキーマの情報は、GraphQL Playgroundと呼ばれるWebツールを使用して自動的にドキュメント化されます。

また、GraphQLには、スキーマを可視化するためのツールが多数存在し、スキーマを可視化したドキュメントを作成することができます。例えば、GraphQLの公式ツールであるGraphiQLや、Voyagerというツールがあります。これらのツールを使用することで、スキーマに含まれるオブジェクト、フィールド、クエリ、ミューテーションなどの情報を可視化し、簡単にAPIドキュメントを作成することができます。

GraphQLのスキーマを可視化するツールには、いくつかの選択肢がありますが、デファクトスタンダードとなっているのはGraphQLの公式ツールであるGraphiQLと、ApolloのVoyagerです。

GraphiQLは、GraphQLのスキーマやクエリを可視化するためのWebベースのIDEで、GraphQLの公式ツールとして知られています。GraphiQLは、スキーマの検索やエラーのデバッグなど、豊富な開発機能を提供しています。

一方、Voyagerは、Apolloによって開発されたGraphQLのスキーマ可視化ツールで、スキーマをグラフィカルに可視化することができます。Voyagerは、スキーマを直感的に理解しやすくするために、ノードとエッジのような視覚的な表現を使用しています。

これらのツールはどちらも優れた機能を持っており、どちらを使用するかは個人の好みや、開発環境によって異なる場合があります。しかし、GraphQLのスキーマ可視化ツールとしては、GraphiQLとVoyagerが一般的に使用されていると言えます。

https://github.com/graphql/graphiql

https://dev.classmethod.jp/articles/try-graphiql/

https://ivangoncharov.github.io/graphql-voyager/

https://github.com/IvanGoncharov/graphql-voyager

----
