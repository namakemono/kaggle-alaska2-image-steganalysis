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
	- YCBCr色空間に関するトレーニング
---

## References

### Notebooks & Discussions

- https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/155392
    - どういう方法が有効か説明してくださっています．
- https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168548
    - 1位解法
- https://www.kaggle.com/c/alaska2-image-steganalysis/discussion/168537
    - 4位解法

## 類似コンペ


