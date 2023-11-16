# **Vision Transformer**
ViT（Vision Transformer）とは、Transformerをimage classificationなどのタスクに適用した手法。TransformerのEncoder（左側の部分）を使用する。ViTでは、入力画像をパッチに分割して、パッチごとの関連性をAttention Headで学習することで、Transformerが単語同士の関連性を学習するのと同様に学習を行っている。小規模なデータセットでは、CNNベースの従来型のモデルの方がベンチマークが良いらしい。大規模なデータセットでは高いスコアを記録している。CLIPでは、画像エンコーダーとしてかつてはCNNベースのResNetを利用していたが、現在ではViTが使われている。

# **Architecture**
![](https://github.com/google-research/vision_transformer/raw/main/vit_figure.png)
1. **Linear Projection of Flattened Patches**
    - 入力は画像であり、これを等間隔で区切られた小さなパッチに分割する。各パッチはベクトル化され、位置情報を加えることでフラットなシーケンスが形成される。

2. **Positional Embeddings**
    - 分割したパッチに対して位置情報を付加する。これにより、モデルがパッチごとの相対的な位置を学習する。

3. **Extra learnable class embedding**
    - 画像の分類を可能にするため、シーケンスの先頭に学習可能なclassトークンを追加する。

3. **Transformer Encoder**
    - Transformer Encoderにパッチ化し、位置情報とclassトークンを付加したベクトルを供給する。ViTのAttention Headはパッチをある種のワードとみなし、Transformerがテキストのワード間の関連性を学習することと同様に、パッチ間の関連性を学習する。

4. **MLP Head**
    - MLPはMulti Layer Perceptronのこと。Softmaxしてクラス予測を行う。