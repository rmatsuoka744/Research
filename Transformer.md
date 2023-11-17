# **Transformer**
最近主流の言語モデルにはたいてい採用されているアーキテクチャ。かつて言語処理の分野ではRNNをベースとしたモデルが採用されていた。しかし、RNNでは系列データを順番に処理するという性質があり並列処理が難しく、計算効率が低かった。また、長い文脈において長期的な依存関係を学習することが困難であり、勾配消失や爆発などの問題があった。これらの問題を解決するため、柔軟で拡張性もあり、並列処理が容易で、Self-Attentionによって長い依存関係を効果的に学習できるTransformerが発展してきた。Transformerは非常に柔軟なアーキテクチャであり、自然言語処理の分野ではMachine Translation、Text Summarization、Q&A、Text Generation、Text Classification、画像処理の分野ではImage captioning、Object detection、Image generationなど様々なタスクに適用できる。

# **Architecture**
![Architecture](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221163853.png)
1. **Input Embedding & Positional Encoding**
   - 入力テキストをEmbeddingで単語埋め込みベクトルに変換した後、Positional Encodingで三角関数からなる位置情報を付け加えることで、単語の種類や順序を保持したベクトルを生成する。単語埋め込みとは、類似した単語を空間上の近い位置に配置し、そうでない単語を離れた位置に配置することでコサイン類似度の計算を可能にし、単語同士の類似度を計算可能にする手法である。

2. **Attention**
![Attention](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221163903.png)
   - Attentionとは、Queryに関連するValueを取り出すための機構である。はじめにTargetからQueryを、SourceからKey、Valueをそれぞれ計算する。QueryとKeyの積を計算し、Softmaxで正規化することで、QueryとKeyの類似度を計算することができる。これをAttentionのweightとし、Valueと積をとることでValueの中からQueryと関連した部分を抜き出すことができる。画像はCross-Attentionの場合であり、Self-AttentionではSourceとTargetを同一のSelfとして計算する。

3. **Multi-Head Attention**
![Multi-Head Attention](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221164455.png)
   - Q、K、Vをそれぞれモデルの次元数/ヘッド数に分割し、Attentionに供給する。モデルの次元数が512、ヘッド数が8なら次元数64のAttentionを8つ計算する。

4. **Add & Norm**
   - ResNetなどで用いられる残差接続と同様に、AttentionをスキップしたベクトルをAttentionを適用したベクトルに加算する。NormはLayer Normalizationであり、レイヤーごとに正規化を行う。

# **Reference**
paper https://arxiv.org/pdf/1706.03762.pdf  
解説 https://deeplearning.hatenablog.com/entry/transformer
