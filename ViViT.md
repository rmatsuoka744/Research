# **Video Vision Transformer**
ViViTは、ViT（Vision Transformer）をベースとした動画データを対象としたタスクを学習するモデルである。動画分類、動作認識、アクション検出など様々なビデオタスクに利用される。ViTで画像をパッチごとに分割したように、ViViTでは動画のフレームごとにパッチ分割を行いTransformerに供給することで学習を行う。ViTとの違いとしては、動画では空間的な位置情報以外にも、時間的な位置情報も必要であるため、空間・時間的な位置情報を組み込むPositional Embeddingを導入する。

# **Architecture**

1. **Embedding video clips**
    - 以下に示すように、2種類の埋め込み手法がある。1.ではそれぞれのフレームを独立した画像として扱い、時間的な関係性はTransformerによって学習する。一方で、2.ではEmbeddingの時点で時間的な関係性を考慮した埋め込みを行う。これは、2DCNNと3DCNNの関係と類似している。
    1. **Uniform frame sampling**
    ![](https://deideeplearning.com/wp-content/uploads/2021/04/image-35-1.jpg)  
    入力動画クリップのすべてのフレームから均等な間隔でフレームをサンプリングし、それぞれのフレームをViTと同様の手法でEmbeddingする。これにより、画像の空間的な位置情報を埋め込んだベクトルがサンプリングしたフレーム数だけ得られる。これらのベクトルを連結することで、ViTに供給するデータを生成する。この手法では、異なるフレーム同士の時空間情報はViTによって融合されることになる。
    2. **Tubelet embedding**
    ![](https://deideeplearning.com/wp-content/uploads/2021/04/image-35-2.jpg)  
    図のように、入力動画をT*H*Wの3D画像のように捉えることで、空間情報と時間情報をまとめて考慮したEmbeddingを行う。図のx1やx2などはTubesと呼ばれる。これらのTubesを線形投影し、ベクトルに変換してから順に並べることで、ViTに供給するデータを生成する。

2. **ViT**
![](https://deideeplearning.com/wp-content/uploads/2021/04/image-35.png)  
    - Embeddingによって生成されたデータはViTに供給される。図にあるように、いくらかのモデルが