（QGISのバージョンは3.16.7で動作確認）

# CityGMLファイルのダウンロード
- G空間情報 https://www.geospatial.jp/ckan/dataset/plateau
- 東京23区のメッシュ番号確認用
    - <a href="https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/40f54174-7c7d-4f9f-b392-8c8ad585b09b" target="_blank">北西エリア</a>
    - <a href="https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/40803cc8-c45c-4a81-bb8b-43ea83e3c04b" target="_blank">北東エリア</a>
    - <a href="https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/780b8865-6642-4537-a6b3-884d259eb591" target="_blank">南西エリア</a>
    - <a href="https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/0ad3e254-0c51-4346-ac9b-204e1077f7a0" target="_blank">南東エリア</a>

# 建物データをQGISに取り込み、gpkg形式に変換
- gmlのままやメモリ上で処理すると、次のXYスワップがうまくいかないため。。。

```
QgsVectorFileWriter.writeAsVectorFormat(original_gml, gpkg_file, "UTF-8")
```

# XY座標をスワップ
- QGISで読み込むと、X座標とY座標が反対になっているため修正が必要

```
processing.run(
	"native:swapxy", {
	"INPUT":gpkg_file,
	"OUTPUT":"memory:"})["OUTPUT"]
```

# 想定最大浸水深のフィールドを追加
- ```value```フィールドの値は```"(3:1.0,2.0,345)"```のような形式の文字列
- ```規模```フィールドの```L2```に対応する浸水深を新たな属性として取得

```
with edit(layer):
	layer.addAttribute(QgsField('浸水深', QVariant.Double, 'double'))
	layer.updateFields()
	for ft in layer.getFeatures():
		if str(ft.attribute('規模')).find("L2") < 0:
			inund_max = 0
		else:
			x = ft.attribute('value')
			if x[1]=="1":
				m = re.search(r"1:([0-9.]+)", x)
			else:
				m = re.search(r":[^,]+,([0-9.]+)", x)
			inund_max = float(m.group(1))
		ft['浸水深'] = inund_max
		layer.updateFeature(ft)
```

- 想定最大浸水深÷建物の高さで色分け
    - 緑（<1％）～赤（>30％）

```
"浸水深" / "measuredHeight" * 100
```

<img src="https://user-images.githubusercontent.com/34636490/122630113-8a909800-d0fc-11eb-84d1-b692ef25a313.png" width=600/>
