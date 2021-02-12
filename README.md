# AirMonitor
Air Quality monitor for M5Stack<BR>
本ページに記載された内容、および プログラムコードは無保証です。

## 概要/Overview
<img src="image/hardware.JPG" width="420"><BR>
M5Stackを使って、空気環境を測定するデバイスを作りました。<BR>
CCS811,BME280という２つのセンサーを使って下記の５項目を測定し、M5Stackの画面に表示します。<BR>
  ||単位|測定範囲|
  |:---:|:---:|:---:|
  |温度/Temperature|℃, 摂氏|-40℃ to +85℃|
  |湿度/Humidity|%|0% to 100%|
  |気圧/Pressure|hPa, ヘクトパスカル|300hPa to 1100hPa|
  |二酸化炭素相当物/eCO2|ppm|400ppm to 8192ppm|
  |総揮発性有機化合物/TVOC|ppb|0ppb to 1187ppb|
  
※測定範囲は、センサーの仕様に基づく
  
測定値はM5Stackの液晶画面に表示され、下記３つの表示モードがあります。
- 現在値モニタ<BR>
  現在の測定値を表示。
- グラフ<BR>
  過去のデータをグラフ表示。
- データ<BR>
  最近の測定値を一覧表示。
  
wifiネットワークは不要で、電源を繋いでおけば動作します。<BR>
在宅や職場などで、スタンドアローンで空気環境をチェックしたいときにどうぞ。

## パーツリスト/Parts list
* M5Stack Basic/GRAY<BR>
  私はGRAYを使いましたが、Basicでもいけるはず。。未確認
* SparkFun Environmental Combo Breakout - CCS811/BME280 (Qwiic)<BR>
  https://www.sparkfun.com/products/14348
* Qwiic Cable - Grove Adapter (100mm)<BR>
  https://www.sparkfun.com/products/15109<BR>
  私はブレッドボードを使いましたが、このケーブルでいけるはず。。未確認
* 適当なUSB Type-C電源<BR>
  M5Stackへの電源供給用。
  
## 組み立て方/How to build
1. Arduino IDEをセットアップ<BR>
  Arduino IDEをインストールしてM5Stackとの通信COMポートを設定した後、必要なライブラリをインストールします。
   - M5Stack by M5Stack<BR>
     https://github.com/m5stack/m5stack
   - SparkFun BME280 Arduino Library by SparkFun Electronics<BR>
     https://github.com/sparkfun/SparkFun_BME280_Arduino_Library
   - SparkFun CCS811 Arduino Library by SparkFun Electronics<BR>
     https://github.com/sparkfun/SparkFun_CCS811_Arduino_Library
2. スケッチのダウンロード<BR>
  本ページからairMon.ino、envList.hをダウンロードした後、"airMon"という名前のフォルダに入れます。
3. ハードウェアの接続<BR>
   - M5Stackとセンサーを、Qwiic Cableにて接続<BR>
     センサーボードにはQwiicコネクタが２つありますが、どちらでも良いみたい。
   - Arduino IDEをインストールしたパソコンのUSBポートに、M5Stackを接続
4. スケッチのコンパイルと、M5Stackへの書き込み<BR>
  airMon.inoをダブルクリックしてArduino IDEにて開いた後、「マイコンボードに書き込む」を実行。  
## 使い方/How to use
### 電源オン、オフ
* M5StackにUSB電源を接続するか電源ボタンを押すと、電源が入ります。<BR>
  「Wait...」と数秒表示されたあと、現在値モニタ画面が表示されます。
* M5StackからUSB電源を外した後に、電源ボタンを２回続けて押すと電源が切れます。<BR>
  USBにて電源が供給されている時に電源を切ることは出来ません。
### 表示モード切り替え
  前面ボタンを押すと、表示モードが切り替わります。
  - 左ボタン： 現在値モニタモード
  - 中央ボタン： グラフモード
  - 右ボタン： データモード
#### 現在値モニタモード<BR>
  <img src="image/monitor.JPG" width="320"><BR>
  最も最近に測定した値が表示されます。<BR>
  右側の棒グラフはCO2レベルを表わしたもので、下記のように色が変化します。<BR>
  基準は、日本産業衛生学会様のホームページを参考にしました。<BR>
  http://jsoh-ohe.umin.jp/covid_simulator/covid_simulator.html<BR>
  |CO2値|グラフ表示色|状態|
  |:---:|:---:|:---:|
  |0ppm ～ 1000ppm|緑|良い|
  |1000ppm ～ 1500ppm|黄色|やや良い|
  |1500ppm ～ 2500ppm|オレンジ|悪い|
  |2500ppm ～ 3500ppm|ピンク|非常に悪い|
  |3500ppm ～ 6000ppm|赤|極めて悪い|
  |6000ppm ～|棒グラフ全体が赤くなります|〃|
#### グラフモード<BR>
  53分グラフ<BR>
  <img src="image/graph2.JPG" width="320"><BR>
  8時間グラフ<BR>
  <img src="image/graph1.JPG" width="320"><BR>
  過去の測定値を折れ線グラフで表示します。<BR>
  中央ボタンを連続して押すと、表示幅が53分/8時間で切り替わります。<BR>
  右端が最も最近の測定値であり、左に行くほど古い測定値です。<BR>
  - 上端<BR>
    各測定値ごとの色見本を表示します。<BR>
    左に寄っている測定値は左のY軸目盛りを、右に寄っている測定値は右のY軸目盛りで表示します。<BR>
    真ん中に、時間幅(53m/8h)を表示します。<BR>
  - 右端、左端<BR>
    グラフ線のY軸目盛りを表示します。<BR>
  - 下端<BR>
    時間目盛りを表示します。<BR>
    右端(0)が最新で、過去の目盛りはマイナス記号で表示します。<BR>
    "-15m"は15分前、"-2h"は2時間前。
#### データモード<BR>
  <img src="image/data.JPG" width="320"><BR>
  最近の測定値を13回分、一覧表示します。<BR>
  オレンジ色のデータは、8時間幅でグラフを表示した時に選択される値です。<BR>
  一番上(No=0)が最も最近に測定した値であり、下に行くほど古い測定値です。<BR>
### その他
  * 何もボタンを押さずに３分経過すると省電力モードになり、バックライトを消灯します。
  * 空気環境の測定は、10秒毎に行っています。

## 技術的なこと/Technical notes
- データ領域は循環して使用するので、リング型のバッファとしています。
- バッファサイズは3006個であり、10秒毎に測定するので10秒x3006回で8.35時間分の測定値を内部メモリーに保存しています。
- 測定値を漏れなく記録するため、測定値を記録する処理と画面を表示する処理は別スレッドに分けています。
- CCS811とBME280が別になったボードをデイジーチェーン接続しても、動作するかもしれないです。<BR>
  その場合は、I2Cアドレス(CSS811_ADDR)の定義を変更すると良いかも。
  
## 参考資料
 - BME280センサー仕様<BR>
  https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/
 - CCS811センサー仕様<BR>
  https://cdn.sparkfun.com/assets/learn_tutorials/1/4/3/CCS811_Datasheet-DS000459.pdf
 - 神楽坂らせんちゃんさんのgithub<BR>
   色々参考にさせていただいてます^^;<BR>
   https://github.com/kagurazakarasen
