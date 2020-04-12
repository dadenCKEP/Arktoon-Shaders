# Arktoon-Shaders / MarkerBlend

## 内容物
* Sensor: カメラです。アバターに入れると、ローカルでのみ有効になります。
* Sensor_Friend: プレハブです。アバターに入れると、Animatorによって有効になり、フレンドでのみ有効になります。
* 0000.png: 32px*32pxのARGBが(0,0,0,0)のテクスチャです。_MainTex使用するとシェーダフォールバックの挙動を変更できます。
* testmarker.png: 32px*32pxのARGBが左半分は(1,1,1,1)、右半分は(0,0,0,0)のテクスチャです。_MainTexや_SensorTexに入れて動作確認に使用します。
* Animationフォルダ: Sendor_Friendに使用するAnimatorとAnimationが含まれるフォルダです。
* MarkerBlendSample.unity: 動作確認用のサンプルアバターが含まれたシーンです。
* *.mat: 動作確認用のマテリアルです。

## 設定方法
### Sensorを使用する場合
自分にはSub Texure、自分以外にはMain Texureが見えます。また、Colorの値がそれぞれテクスチャに乗算されます。

### Sensor_Friendを使用する場合
フレンドにはSub Texure、フレンド以外にはMain Texureが見えます。また、Colorの値がそれぞれテクスチャに乗算されます。

## 補足(シェーダフォールバックについての詳細)
Safetyシステムによるシェーダフォールバックを食らった場合、Unlit+CutoutやUnlit+Fadeを想定しており、シェーダ名の後ろに``_BtUnlit``をつけて意図的にUnlitへ誘導しています。

MainTexが引き継がれますが、Colorはフォールバック後のシェーダには適用されないみたいです。
シェーダフォールバックを食らった場合にCutoutしたい場合は、Colorを(*, *, *, 0)にするの"ではなく、_MainTexのスロットに付属の0000.pngを使用してください。

## 設定例
### ベースのシェーダを選択
* Opaque・AlphaCutout → AlphaCutout
* Fade → Fade

※ 当然どちらも透明系シェーダに属する。

### 対象選択
* フレンドとフレンド以外で挙動を変える → アバターの適当な位置(Armatureと並列など。どこでもいよい。)にSensor_Friendを入れる
* 自分と自分以外で挙動を変える → アバターの適当な位置(Armatureと並列など。どこでもいよい。)にSensorを入れる

### 自分側の挙動選択
基本的には通常のシェーダと同様で、Sub TextureとSub Colorに入れる。

### 他人側の挙動選択
* 不可視にしたい → Main Textureに付属の0000.pngを入れる
* その他 → Main TextureとMain Colorに入れる。

### Fallback時の挙動
参考: Shader Blocking System (https://docs.vrchat.com/docs/shader-fallback-system)
* MarkerBlend/AlphaCutout_BtUnlit → Unlit/AlphaCutoutでMain Texture(_MainTex)のみ引き継がれる
* MarkerBlend/Fade_BtUnlit → Unlit/FadeでMain Texture(_MainTex)のみ引き継がれる
