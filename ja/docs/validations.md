## バリデーション

モデルのクラスにはバリデーションを設定することが可能です。バリデーションによって、モデルに保存することができるデータの種類を制限することができます。特にバリデーションが有効なのは ```store``` コレクションに対してですが、他でも利用することができます。

現在のところ、2種類 ( length と presence ) のバリデーションが実装されています。これから沢山のバリデーションを追加していく予定です。

```ruby
    class Info < Volt::Model
      validate :_name, length: 5
      validate :_state, presence: true
    end
```

バリデーションがある場合、ここでモデルに対して save を実行すると、以下のようになります:

1. クライアントサイトでバリデーションが実行されます。もしバリデーションがエラーになった場合、```save!``` の結果の promise がエラーオブジェクトを伴って reject されます。2. データはサーバーに送られ、クライアントとサーバーサイドのバリデーションがサーバー上で実行されます。すべてのエラーが返され、promise はフロントエンドで (エラーオブジェクトを伴って) reject されます。
    - バリデーションがサーバーサイドでも再度実行されることで、バリデーションにパスしないデータが保存されてしまうことを確実に回避します。すべてのバリデーションにパスすると、データはデータベースに保存され、promise はクライアント上で resolve されます。4. データが他のすべてのクライアントと同期されます。