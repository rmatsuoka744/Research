# 工事中

# **MobileNet**
TONE Mobileで利用しているNSFW image detectionが採用しているモデル。CNNベースのモデルで、モバイル端末上でも動作するようにパラメータ数を削減したモデルである。現在はMobileNetv3まで公開されているが、TONEではMobileNetv2で学習を行ったNSFW detectorを利用している。

# **MobileNetv1**
1. **Depthwise Separable Convolution**
    - 通常の畳み込みをDepthwise ConvolutionとPointwise Convolutionに分解して、計算コストを削減する。

# **MobileNetv2**
1. **Inverted Residuals**
    - Inverted Residualsと呼ばれる新しい残差ブロックが導入され、モデルの安定性が向上した。

# **MobileNetv3**
1. **Squeeze-and-Excitation**