# ALASKA2 Image Steganalysis

- https://www.kaggle.com/c/alaska2-image-steganalysis

## 概要

- 画像内に隠しメッセージが埋め込まれているかどうかを判定するコンペ
- 埋め込むアルゴリズムは3種類
    - JMiPOD


---

## 評価指標

- AUC

## データ

- 元画像75kと埋め込みアルゴリズムごとに75k

### 制約条件

---

## 方針

- 元画像 or どの埋め込みアルゴリズム化を判定する多クラスのネットワークを作って判定
- EfficientNetが有効.
- Augmentationは反転や90度回転など情報を潰さない手法のみ有効

## 知見

cf. https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/155392

- ❌やっちゃだめなこと
	- 画像のサイズ変更
	- Flip & Rotation
	- 切り取り
- ✅重要なこと
	- 正規化
	- 学習済みのResNet34で色々と試してみると早い
	- EfficientNetが有効(cf. https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168542)
		- Block #5,6を取り外して，Conv2D x 3を先頭に追加すると性能改善
	- TTA
	- 多クラスで解くほうが2値クラスで解くより若干性能が良い
- ❓不明なこと
	- YCbCr色空間に関するトレーニング
        - note: JPEGで保存する色はRGBではなくYCbCr
---

## References

### Top Solutions

- 1: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168548
    - Models: YCbCrとDCTで学習. SE ResNet 18
        - 8x8のDCTコンポーネント(512x512x3 => 64x64x192にDCT空間に変換)
            - cf. http://www.ws.binghamton.edu/fridrich/Research/OneHot_Revised.pdf
    - Augmentations: Rotation 90, Flip, CutMix
    - Training: 65000x4/10000x4(train/validation). 3,4,6,7にデータを分割して学習
    - note: DCTモデルの性能は低い(〜0.87)が，YCbCrモデルとのアンサンブルで性能が改善
- 2: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168546
    - Models
        - EfficientNet
            - B6,B7
            - activationをSwishからMishに変更
                - cf. https://arxiv.org/ftp/arxiv/papers/1908/1908.08681.pdf
    - Augmentation
        - Dropout
        - D4 augmentation(90度ごとに回転させてTTA)
            - cf. https://github.com/BloodAxe/pytorch-toolbelt/blob/d8a7d25c887c5f1c9a6c8e07e8b887bc6fc4617c/pytorch_toolbelt/inference/tta.py#L154
    - Loss:
        - BCE, CEが最善
    - ❌
        - DCTでの学習
        - ResNet & DenseNet
- 3: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168870
- 4: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168537
- 8: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168519
- 9: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168608
    - Models: EfficientNet x 5
        - stride:(1,1)が重要
    - Augmentations: flip & 転置
- 12: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168507
	- Models: EfficientNet x 4, dropout:0.2, concat pooling, AdamW, cross entropy
	    - stride:(1,1)
    - ❌TTA, ResNet/ResNeSt(1位はSE ResNetを使用)
    - env: RTX6000
- 14: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168611
- 18: https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168771

### 類似コンペ


