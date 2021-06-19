# CityGMLファイルのダウンロード
- g空間情報 https://www.geospatial.jp/ckan/dataset/plateau
- <a href="https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/40f54174-7c7d-4f9f-b392-8c8ad585b09b" target="_blank">東京23区拡大図1</a>
  - 都区部北西
- 東京23区拡大図2
# gpkgに変換
- gmlのままやメモリ上で処理すると、次のXYスワップがうまくいかない。。。
```
QgsVectorFileWriter.writeAsVectorFormat(original_gml, gpkg_file, "UTF-8")
```
# XY座標をスワップ
```
processing.run(
	"native:swapxy", {
  "INPUT":gpkg_file,
  "OUTPUT":"memory:"})["OUTPUT"]
```

# 想定最大浸水深のフィールドを追加
```
with edit(layer):
	layer.addAttribute(QgsField('INUNDATION_MAX', QVariant.Double, 'double'))
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
		ft['INUNDATION_MAX'] = inund_max
		layer.updateFeature(ft)
```
