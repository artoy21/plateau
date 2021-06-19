# Qgis2threejsによる可視化
- Qgis2threejsプラグインをインストール
- Exporterを起動
  - Object type: ```Extruded```
  - Height: ```measuredHeight * 1.5```
- たしかにうちから見える景色。。。<br/><img src="https://user-images.githubusercontent.com/34636490/122405678-3500a200-cfbb-11eb-953a-27c46b5052ca.png" width=800/>
- 想定最大浸水深で色分け
  - Color: ```color_rgb(255, 255, 255* (1- min(max("INUNDATION_MAX" ,0)/10, 1)))```
<img src="https://user-images.githubusercontent.com/34636490/122628978-94ae9880-d0f4-11eb-9e7d-3fc40613691c.png" width=800/>
