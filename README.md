# grrm2xtb

## 概要
GrimmeのXTB [https://github.com/grimme-lab/xtb] をGRRM [https://iqce.jp/GRRM/] の量子化学計算エンジンとして利用するためのPythonスクリプトです。GRRM17で動作確認してある程度まっとうに動いていることを確認していますが、量子化学計算は全然専門でもないので動作の正しさの保証は特にありません。特にFrozenAtomsやMicroiterationが呼ばれたときの動作は自信がないです。（自分で使っている限りはまっとうに動いていますが……）

## 設定

 - XTBは呼べるようにパスを通しておきます。
 - grrm2xtb ファイルも呼べるように適当な場所におくか、パスを通しておきます。
 - grrm2xtb のシバンは適宜書き換えます。

## GRRMのジョブの書き方、計算条件の設定

先頭とOptionsセクションに次のように設定します。計算方法や基底関数の部分はなしです。電荷と多重度も実際の計算には反映されません。これらは後述するように環境変数で設定してXTBに渡します（この情報をきれいにスクリプトから取ってくるが難しかったのでそうしています）
```
%link=non-supported
#MIN

電荷・多重度・座標
Options
色々
sublink=grrm2xtb
```

電荷・多重度・溶媒・XTBのパラメータのバージョン（GFN1かGFN2か）は GRRM実行用のジョブスクリプトなどから、以下の環境変数で設定します。設定されてない場合、電荷0、多重度1、溶媒なし、GFN2 (これはXTBのデフォルトが利用) で計算されます。

```
export XTB_CHARGE=0
export XTB_MULTI=1
export XTB_SOLVENT=CH2Cl2
export XTB_PARAM=2
```

後は普通にGRRMを実行してください。ログに出力される内容のうち DIPOLE、POLARIZABILITY、S**2などは全部0として出力します。

