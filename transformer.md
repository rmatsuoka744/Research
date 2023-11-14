# **Transformer**
最近主流の言語モデルに採用されているアーキテクチャ。大規模なデータセットを対象に学習を行う場合に適しているらしい。Text Classification、Q&A、Summarization、Image2Text、Image Classification、Instructionなど様々なタスクに適用できる。BERT、GPT、CLIP、BLIPなど様々なモデルに採用されている。

# **Architecture**
![Architecture](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221163853.png)
1. **Input Embedding & Positional Encoding**
   - 入力テキストを単語埋め込みベクトルに変換した後、三角関数で計算する位置情報を付け加えることで、Transformerに入力できる形にデータを加工する。単語埋め込みは、類似した単語を空間上の近い位置に配置し、関係のない単語を離れた位置に配置することでコサイン類似度の計算を可能にし、単語同士の類似度を計算可能にする手法。

2. **Attention**
![Attention](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221163903.png)
   - Attentionとは、Queryに関連するValueを取り出すための機構である。はじめにTargetからQueryを、SourceからKey、Valueをそれぞれ計算する。QueryとKeyの積を計算し、Softmaxで正規化することで、QueryとKeyの類似度を計算することができる。これをAttentionのweightとし、Valueと積をとることでValueの中からQueryと関連した部分を抜き出すことができる。画像はCross-Attentionの場合であり、Self-AttentionではSourceとTargetを同一のSelfとして計算する。

3. **Multi-Head Attention**
![Multi-Head Attention](https://cdn-ak.f.st-hatena.com/images/fotolife/R/Ryobot/20171221/20171221164455.png)
   - Q、K、Vをそれぞれモデルの次元数/ヘッド数に分割し、Attentionに供給する。モデルの次元数が512、ヘッド数が8なら次元数64のAttentionを8つ計算する。

4. **Add & Norm**
   - ResNetなどで用いられる残差接続と同様に、AttentionをスキップしたベクトルをAttentionを適用したベクトルに加算する。NormはLayer Normalizationであり、レイヤーごとに正規化を行う。

# 参考
論文 https://arxiv.org/pdf/1706.03762.pdf  
解説 https://deeplearning.hatenablog.com/entry/transformer