# Qgis2threejsによる3D表示
- Qgis2threejsプラグインをインストール
- Exporterを起動
  - Object type: ```Extruded```
  - Height: ```measuredHeight * 1.5```
- たしかにうちから見える景色。。。
<img src="https://user-images.githubusercontent.com/34636490/122405678-3500a200-cfbb-11eb-953a-27c46b5052ca.png" width=800/>

- 想定最大浸水深で色分け
  - Color: ```color_rgb(255, 255, 255 * (1- min("浸水深" / 10, 1)))```
    - RGBの設定式は適当
<img src="https://user-images.githubusercontent.com/34636490/122630335-c415d300-d0fd-11eb-9d2e-70086002c701.png" width=800/>
