# **Stable Diffusion**
- Stable DiffusionはDiffusion Modelをベースとしたtext2imageの画像生成モデルである。

# architecture
- Stable Diffusionは4つのコンポーネントから構成されている。ピクセル画像をエンコードするVAE Encoder、入力テキストをエンコードするCLIP、エンコードされた入力データから画像を生成する拡散モデルのU-Net、そしてU-Netが生成したデータをデコードするVAE Decoderである。

1. **VAE(Variational Autoencoder)**
    - VAEは画像をピクセル画像から画像埋め込み表現に変換する。StableDiffusionでは512\*512サイズの入力画像から8\*8サイズの潜在表現に変換される。

2. **CLIP Text Encoder**
    - 画像と同様に、テキストからも埋め込み表現を入手する必要があるため、Transformerをベースとする学習済みCLIPからText Encoderを使用する。

3. **Diffusion Model**
    - ノイズ画像からデータを生成することは難しい反面、画像に徐々にノイズを加えていくことでノイズ画像を生成することは容易である。StableDiffusionでは画像に正規分布に従うノイズを加える操作のことを拡散プロセス(forward diffusion process)と呼び、その逆であるノイズを除去する操作を逆拡散プロセス(reverse diffusion process)と呼ぶ。ここではVAE Encoderから得た潜在表現に拡散プロセスを施し、U-Netによって逆拡散プロセスを学習することで画像生成を実現する。

# Training
StableDiffusionでは、入力画像から得た潜在表現とU-Netが復元した洗剤表現の誤差を損失として、逆拡散プロセスを行うU-Netのパラメータを更新することで学習を行う。入力画像はVAE Encoderによって潜在表現に変換された後、正規分布にしたがって拡散プロセスを適用する。U-Netは逆に、ノイズに変換されたデータを元の潜在表現に変換することを目的として学習していく。復元された潜在表現はVAE Decoderによってピクセル画像に変換される。

# Evaluate
StableDiffusionでは、推論の際にユーザーからの入力テキストを利用する。はじめ、拡散モデルはランダムシードからノイズを生成し、ノイズとテキストの埋め込み表現をともにU-Netに供給することでテキストによって条件付けされて復元された潜在表現を得ることができる。この潜在表現をVAE Decoderによってデコードすることで、生成画像を得る。

# StableDiffusionでのU-Net
通常のU-NetはResNetのブロックをUの字状の階層構造に配線したようなモデルである。これは畳み込み演算をベースとしたモデルであり、入力された特徴マップを畳み込みによってダウンサンプリングしていき、その後同じ回数だけ逆畳み込みを行うことでアップサンプリングを行う。セグメンテーションタスクなどで利用されるモデルである。StableDiffusionにおいては、入力データとして画像の特徴マップだけでなく、テキストの埋め込み表現も用いるため、ResNetのブロックの後ろにAttentionブロックを配線したモデルを使用している。Attentionブロックでは、画像の特徴マップとCLIP Encoderからの単語埋め込み表現がCross-Attentionされる。