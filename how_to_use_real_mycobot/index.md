
# 実機の使い方 - myCobot の場合

実機と MoveIt! を使って
プログラムからロボットを操作する際の
実機の使い方を説明します．

<a id="start-mycobot-moveit-real-robot"></a>

## <a href="#start-mycobot-moveit-real-robot">myCobot280 の場合 </a>

### myCobot280 の固定

myCobot280がぐらつかないように，固定をしてください．

- 土台となる板を用意し，myCobot をネジで固定．

- 土台となる板自体がぐらつかないように，机と固定．または，大きな板を使用してください．

![myCobot - how to fix a robot](figs/mycobot-how-to-fix-a-robot.png)

- [Flat Stand](https://docs.elephantrobotics.com/docs/gitbook-en/2-serialproduct/2.7-accessories/2.7.1-fsta.html)や[Gベース](https://docs.elephantrobotics.com/docs/gitbook-en/2-serialproduct/2.7-accessories/2.7.1-fsta.html)のような固定用ベース部品を利用することもできます．

### myCobot280 のファームウェア更新

#### myStudio のダウンロード

Windows PCが必要です．

[MyStudio の リリースページ](https://github.com/elephantrobotics/MyStudio/releases)
にアクセスし，
最新の exe ファイルをダウンロード・実行してください．

![myCobot - MyStudio startup screen](figs/mycobot-mystudio-startup-screen.png)

#### USB ドライバのインストール

Windows/VMware上のUbuntuからmyCobotを制御する場合、Windows側のドライバーをインストールしない場合VMwareからうまく認識されない場合がありますので、以下のUSBドライバをインストールしておいた方が良いでしょう。

[SILICON LABS CP210x USB - UART ブリッジ VCP ドライバ](https://jp.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads)
にアクセスし，
CP210x Windows Drivers をダウンロードしてください。
ZIPファイルを展開して、CP210xVCPInstaller_x64.exe を実行します。

#### ファームウェアの更新

アーム先端の M5Stack Atom と胴体の M5Stack Basic のファームウェアを更新します．

**M5Stack Atom のファームウェア更新**

今回は，AtomMain の v4.1 をインストールします．

MyStudio を立ち上げた後，
アーム先端の Atom とパソコンを接続してください．

MyStudio の画面で USB Port の欄が ATOM になったはずです．

![myCobot - MyStudio screen shows usb port is connected to Atom](figs/mycobot-mystudio-screen-shows-usb-port-is-connected-to-atom.png)

その状態で接続した後，Basic タブをクリックし，AtomMain の v4.1 を選択してインストールします．

![myCobot - installation AtomMain v4.1 from MyStudio](figs/mycobot-installation-atommain-v4-1-from-mystudio.png)

**M5Stack Basic のファームウェア更新**

今回は，minirobot の v1.0 をインストールします．

MyStudio を立ち上げた後，
胴体の Basic とパソコンを接続してください．

MyStudio の画面で USB Port の欄が BASIC になったはずです．

![myCobot - MyStudio screen shows usb port is connected to Basic](figs/mycobot-mystudio-screen-shows-usb-port-is-connected-to-basic.png)

その状態で接続した後，Basic タブをクリックし，minirobot の v1.0 を選択してインストールします．

![myCobot - installation minirobot v1.0 from MyStudio](figs/mycobot-installation-minirobot-v1-0-from-mystudio.png)

**動作確認（ティーチングデモ）**

Basic とパソコンを接続し，
myCobot の画面 (miniroboFlow) から Maincontrol を選択してください．

![myCobot - teaching demo: select Maincontrol from miniroboFlow](figs/mycobot-teaching-demo-1-select-maincontrol-from-miniroboflow.png)

動作のティーチングを行うために，Record を選択してください．

![myCobot - teaching demo: select record mode](figs/mycobot-teaching-demo-2-select-record-mode.png)

Recording to Ram/Flash? と聞かれるので，Ram を選択してください．

![myCobot - teaching demo: select Ram from record mode](figs/mycobot-teaching-demo-3-select-ram-from-record-mode.png)

同様に，Play で教えた動作を再生します．

### myCobot280 を Transponder モードにする

以降，パソコンから myCobot に指令を送る際には，
myCobot の画面 (miniroboFlow) から Transponder を選択してください．

![myCobot - Transponder mode](figs/mycobot-transponder-mode.png)

### dialout グループへのユーザ追加

シリアルポートにアクセス権を持つ，
dialoutグループにユーザを追加します．

**ターミナル**

```bash
sudo adduser $USER dialout 
```

### pymycobot (Python API) のインストール

myCobot を Python で動かすための API をインストールします．

**ターミナル**

```bash
pip install pymycobot --user
```

**動作確認**

```python
In [1]: from pymycobot.mycobot import MyCobot
In [2]: mycobot=MyCobot('/dev/ttyUSB0')
```

```python
In [3]: mycobot.set_color(0,0,255)
```

![myCobot - pymycobot API: mycobot.set_color(0,0,255)](figs/mycobot-pymycobot-api-setcolor-0-0-255.png)

```python
In [4]: mycobot.set_color(0,255,255)
```

![myCobot - pymycobot API: mycobot.set_color(255,0,255)](figs/mycobot-pymycobot-api-setcolor-0-255-255.png)

```python
In [5]: from pymycobot.genre import Angle
In [6]: mycobot.send_angles([0,0,0,0,0,0], 80)
```

![myCobot - pymycobot API: mycobot.send_angles([0,0,0,0,0,0], 80)](figs/mycobot-pymycobot-api-send-angles-to-reset-pose.png)

<div style="text-align: center;">
    <a href="../rosset_simulator"><strong>◀[前]「NEDO ROSセットのシミュレータの利用」</strong></a>
    ・
    <a href="../program_basic"><strong>「MoveIt!プログラミングの基礎」[次]▶</strong></a>
</div>

<!-- EOF -->
