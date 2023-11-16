# **CLIP（Contrastive Language-Image Pretraining）**
2021年にOpenAIから発表されたモデル。言語と画像の間の相互作用を理解し、異なるタスクで利用することを目的としている。Image Classification、Image to text、zero shot learningなど様々なタスクに適用可能である。

# **Architecture**
![](https://github.com/openai/CLIP/raw/main/CLIP.png)

CLIPは言語と画像の相互作用を利用するマルチモーダルなアーキテクチャである。画像から特徴量を抽出するImage Encoder、テキストから抽出するText Encoderをそれぞれ学習する。言語と画像のペアが入力として与えられ、それぞれの特徴量を比較する。

1. **Image encoder**
    - 画像から特徴量を抽出する。かつてはCNNベースのResNetを利用していたが、現在ではViTを利用する。

2. **Text Encoder**
    - Transformerを利用して、テキストから特徴量を抽出する。

3. **Contrastive pre-training**
    - 画像とテキストのペアをN個用意した時、正解のペアである正の例がN個、不正解のペアである負の例がN^2-N個できる。CLIPでは、正の例同士の特徴量のコサイン類似度を高く、負の例同士のコサイン類似度を低くするように学習する。これは画像の（1）にあるように、二つの特徴量のテンソル積のうち、対角線上のものを最大に、それ以外を最低になるように学習することに言い換えられる。

4. **Create dataset classifier from label text**
    - ゼロショット学習を行うため、予測クラスの候補を作成する。この際、plane,car,dogなどをそのままEncodeするより、A photo of a plane、などとする方が精度が向上する。

5. **Zero-Shot prediction**
    - 入力画像の特徴量と、クラスの候補テキストの特徴量とのコサイン類似度を計算することで、クラスの予測を行う。

# **Reference**
paper https://arxiv.org/pdf/2103.00020.pdf  
github https://github.com/openai/CLIP  
解説 https://deepsquare.jp/2021/01/clip-openai