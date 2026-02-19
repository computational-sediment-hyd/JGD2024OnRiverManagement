## 測地成果2024（JGD2024）の河川管理への影響（補正量計算Pythonスクリプト付き）

### 測地成果2024とは(Gemini、一部修正)

（参考：[国土地理院 基準点の新しい測量成果「測地成果2024」等の提供を開始します](https://www.gsi.go.jp/keikaku/hyoko2024rev-data.html)）

「測地成果2024」は、国土地理院が**2025年4月1日から導入**した、日本の新しい測量の基準となる成果です。


一言でいうと、**これまで水準測量（歩いて測る）をベースにしていた『高さ』の基準を、衛星測位（GNSS/GPS）をベースにする仕組みへとアップデートしたもの**です。

概要を分かりやすくまとめました。

---

#### 1. 何が変わったのか？

最大の特徴は、**水平（緯度・経度）は変わらず、標高（高さ）だけが変わった**点です。

* **標高の全面改定:** 全国の電子基準点、三角点、水準点の標高値が新しくなりました。
* **ジオイド・モデルの刷新:** 新しい「ジオイド2024」が導入されました。これにより、人工衛星で測った高さ（楕円体高）から、より正確に「標高」を計算できるようになりました。
* **水平座標は維持:** 日本測地系2011（JGD2011）の水平位置は引き継がれるため、緯度・経度自体に大きな変更はありません。

<img src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/fig/fig_gsiHP.png" width="40%">

水準点改定量のコンター図（測地成果2024移行のための水準点標高補正パラメータ）コンター間隔：5cm (出典：[国土地理院webサイト](https://www.gsi.go.jp/sokuchikijun/hyoko2024rev.html))

#### 2. なぜ改定されたのか？

理由は大きく2つあります。

* **蓄積したズレの解消:** 東日本大震災以降の地殻変動や、従来の測量方法（日本水準原点から遠くなるほど誤差がたまる性質）による「現況とデータのズレ」をリセットするためです。
* **測量の効率化:** これまでは高い精度で標高を出すには、重い機材を持って歩く「水準測量」が必要でした。今後はGNSS（衛星）を使って、短時間で精度の高い標高を取得できる「GNSS標高測量」が本格普及します。

#### 3. 私たちの生活や業務への影響

* **山の高さが変わった:** 富士山など、日本の主要な山の標高が数センチ〜数十センチ単位で修正されました。
* **工事・測量現場:** 公共測量では2025年4月以降、この「測地成果2024」を使うことがルールとなりました。古いデータ（2011）と混ぜて使うと、高さがズレてしまうため注意が必要です。
* **地図データの更新:** Googleマップなどの民間地図も、今後順次この新しい基準に合わせて更新されていくことになります。

---

#### 補足：名称の整理

* **測地系の名称:** 日本測地系2024（JGD2024）
* **成果の名称:** 測地成果2024
* **モデルの名称:** ジオイド2024

### JGD2011からJGD2024への標高補正方法

#### 標高変換ツール(JGD2011からJGD2024)

JGD2011からJGD2024への変換は、基本的には国土地理院が提供している以下のツールを使用する。

##### Web版PatchJGD(標高版) 

 - [水準点標高補正用 Ver.1.0.1](https://vldb.gsi.go.jp/sokuchi/surveycalc/patchjgd_BMh2024/index.html)
 - [三角点標高補正用 Ver.1.0.1](https://vldb.gsi.go.jp/sokuchi/surveycalc/patchjgd_TRh2024/index.html)

※従来の地殻変動を補正する[PatchJGD標高版 Ver.1.0.1](https://vldb.gsi.go.jp/sokuchi/surveycalc/patchjgd_h/index.html)では計算できません。

##### ソフトウェアPatchJGD_HV

 - [座標標高補正ソフトウェア　PatchJGD HV](https://www.gsi.go.jp/sokuchikijun/sokuchikijun41010.html)


#### 標高補正パラメータファイル

現時点（2026年1月）では、Ver.1.0.0が最新です。[公式サイト](https://www.gsi.go.jp/sokuchikijun/sokuchikijun41012.html)より、「測地成果2024移行のための水準点標高補正」または「測地成果2024移行のための三角点標高補正」のパラメータファイルをダウンロードして下さい。圧縮ファイルを解凍するとパラメータファイルが入手できます。

### 標高補正の河川縦断形への影響

#### サンプルデータ

負の標高補正量が比較的大きい東北地方の日本海側の河川である米代川（河川コード:8202080001）の左岸堤防高の縦断形状を[サンプルデータ](https://raw.githubusercontent.com/computational-sediment-hyd/JGD2024OnRiverManagement/refs/heads/main/kui_leftbank_with_elevation.geojson)とします

<div style="width:600px;height:299px;">
<div style="width:630px;">
<iframe src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/kui_leftbank_elevation.html"
frameborder="0" width="630" height="315" style="transform:scale(0.93); transform-origin: left top;">
</iframe></div></div>

図 標高点位置

#### Web版PatchJGD(標高版)による変換

 - 水準点標高補正用を使用
 - 平面直角座標系を使用
 - 入力ファイル[xy_h.in](https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/xy_h.in)のフォーマット以下のとおり


```sh
#  X(m)          Y(m)       標高(m)   点名
-229928.6536   82441.4953   122.945 襟裳岬灯台 12
-227378.9898   81408.3535    22.260 水準点A    12
-230000.0000   86000.0000     0.000 海上       12
```
 - GIS等とx, yが逆になることに注意。
 - 標高(m)より右のデータは無くてもO.K.
 - データ数は最大30000万点まで

参考として、サンプルデータから入力ファイルを作成するためのPythonスクリプトは以下のとおり

```python
import numpy as np
import pandas as pd
import geopandas as gpd

gdf = gpd.read_file('kui_leftbank_with_elevation.geojson')
coords = np.array([g.coords[:][0] for g in gdf.geometry.values])
X = coords[:, 0]
Y = coords[:, 1]
Z = gdf['Elevation'].values

with open('xy_h.in', 'w', encoding='utf-8') as f:
    f.write("# X(m) Y(m) 標高(m)\n")
    for x, y, z in zip(X, Y, Z):
            f.write(f"{y} {x} {z}\n")

```

[水準点標高補正用 Ver.1.0.1](https://vldb.gsi.go.jp/sokuchi/surveycalc/patchjgd_BMh2024/index.html) にアクセスし、下のとおりに実行すると補正量を示した[xy_h.out](https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/xy_h.out)が作成される。


<img src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/fig/sample01.png" width="50%">

#### ソフトウェアPatchJGD_HVによる変換

 - ソフトウェアをインストールし、パラメータファイルをダウンロードしておく。 
 - 水準点標高補正用を使用
 - 平面直角座標系を使用
 - 入力ファイル[input.txt](https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/input.txt)のフォーマット以下のとおり

```sh
#X(m) Y(m) 座標系番号 標高（m） コメント
-7673.629 -24790.035 2 64.86 田島
-23180.498 6439.512 02 1101.02 高千穂
-18214.881 -7032.804 2 409.07 大峰 
```
 - GIS等とx, yが逆になることに注意
 - コメントは無くてもO.K.


参考として、サンプルデータから入力ファイルを作成するためのPythonスクリプトは以下のとおり

```python
import numpy as np
import pandas as pd
import geopandas as gpd

gdf = gpd.read_file('kui_leftbank_with_elevation.geojson')
coords = np.array([g.coords[:][0] for g in gdf.geometry.values])
X = coords[:, 0]
Y = coords[:, 1]
Z = gdf['Elevation'].values

with open('input.txt', 'w', encoding='shift-JIS') as f:
    f.write("# X(m) Y(m) 座標系 標高(m)\n")
    for x, y, z in zip(X, Y, Z):
            f.write(f"{y} {x} 10 {z}\n")

```

ソフトウェアで下のとおりに入力し、「補正結果出力」をクリックすると、出力ファイル[.out](https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/out.out)が作成される。また、画面上に補正量が図化される。

<img src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/fig/sample012.png" width="50%">

参考：高速化のテクニック

ソフトウェアPatchJGD_HVは演算速度がかなり遅いです。点数に制限は無いですが相当な時間がかかります。

以下のとおり、画面表示をオフにすると若干早くなります。

<img src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/fig/speedup.png" width="50%">

出典：ジオ・コーチ・システムズwebサイト https://geocoach.co.jp/help/DMPatchJgdTransform0Dialog.pdf

#### 標高補正の河川縦断形への影響評価

 - JGD2011、JGD2024の標高および両者の差分（補正量）の縦断図を下図に示します。
 - 約70kmの区間で補正量は-0.21～-0.26m程度で上流に向かって大きくなっています。
 - 河川の縦断スケールで見た場合、全体的に-0.20m程度スライドするだけで、影響はほとんどないと思われます。

<div style="width:600px;height:598px;">
<div style="width:630px;">
<iframe src="https://computational-sediment-hyd.github.io/JGD2024OnRiverManagement/kui_leftbank_elevation_plot.html"
frameborder="0" width="630" height="630" style="transform:scale(0.93); transform-origin: left top;">
</iframe></div></div>

### 補足：標高補正プログラムの作成（Python、暫定版）

大量のデータを処理する場合、上記の方法では手間がかかるので自作コードで計算したほうが早いです。

暫定的にPythonコードを書きましたので必要な方は参考程度にお使い下さい。

コードはGitHub、nbviewer、Colabで公開しています。

#### パラメータファイルをメッシュコードから緯度経度に変換

[![a](https://raw.githubusercontent.com/computational-sediment-hyd/logo/main/mark-github_s1.svg)](https://github.com/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/00parameterfile2geoparquet.ipynb)
[![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.org/github/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/00parameterfile2geoparquet.ipynb)
[![colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/00parameterfile2geoparquet.ipynb)

 - パラメータファイルはhyokorevBM_jgd2024_h.parのみ対応
 - 参考：メッシュコードについて https://club.informatix.co.jp/?p=1226


#### 補正量を計算

[![a](https://raw.githubusercontent.com/computational-sediment-hyd/logo/main/mark-github_s1.svg)](https://github.com/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/01CorrectionValuesUsingBilinearInterpolation.ipynb)
[![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.org/github/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/01CorrectionValuesUsingBilinearInterpolation.ipynb)
[![colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/computational-sediment-hyd/JGD2024OnRiverManagement/blob/main/pyJGD2011toJGD2024/01CorrectionValuesUsingBilinearInterpolation.ipynb)

 - 地理院公開ツールと比較すると0.数mm程度ずれます。打ち切り誤差の問題と思われます。
 - サンプルデータの誤差（上記コードの出力値 - Web版PatchJGD(標高版)出力値）は次のとおりです。
    - 平均誤差: 5.7685542693743294e-06 
    - 最大差: 0.0004983876454069114
    - 最小差: -0.0004998050246612651

