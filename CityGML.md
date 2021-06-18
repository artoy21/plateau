# CityGMLファイルのダウンロード
- g空間情報<br/>
https://www.geospatial.jp/ckan/dataset/plateau
- 東京23区拡大図1 https://www.geospatial.jp/ckan/dataset/plateau-tokyo23ku-citygml-2020/resource/40f54174-7c7d-4f9f-b392-8c8ad585b09b
  - 都区部北西
- 東京23区拡大図2
# gpkgに変換
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
