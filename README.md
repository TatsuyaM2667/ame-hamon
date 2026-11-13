# ame-spectrum

今鳴っている音のスペクトラムを 1 行ずつ流す小さなコマンド(Odin。cava の代わり)。

[ame-shell](https://github.com/TatsuyaM2667/ame-shell)(Wayland のデスクトップシェル)の一部を、単独で使えるように切り出したものです。
ame-shell は画面の要らない仕事を、1 つのことをして標準入出力でやりとりする小さなコマンドに分けています。これはその 1 つです。

## 使い方

```text
ame-spectrum: 今鳴っている音を周波数ごとの棒の高さにして、1 行ずつ流す(cava の代わり)。

  ame-spectrum [-b 本数]     1 行 = 棒の高さ(0〜1000)を ';' でつないだもの(cava の raw ascii と同じ形)

音は PulseAudio の録音の口(PipeWire なら pipewire-pulse)の @DEFAULT_MONITOR@ から、libpulse-simple を実行時に読んで取る。
計算: 窓をかけて FFT(anim/fft.odin。ame-shell と同じもの)、50Hz〜12kHz を対数で分け、dB にして最近の大きい音で
自動でそろえる。上がるときはすぐ、下がるときはなめらかに。1 秒に約 60 行。
全部 0 の行は続けて出さない(無音のとき、読む側を起こさない)。
終了コード: 0 読む側が閉じた / 1 音を取り込めない
```

## ビルド

```sh
sh build.sh   # ./ame-spectrum(Odin dev-2026-08 以降。libpulse などは実行時に読むので、ヘッダも -dev パッケージも要らない)
odin test .
```

実行に要るもの: `libpulse-simple`

## このリポジトリについて

正本は ame-shell の `odin/ame-spectrum/main.odin` です。このリポジトリは `scripts/split-repos.sh` が生成して同期しています。
直したいところがあれば ame-shell の方へお願いします。

ライセンス: GPL-3.0(ame-shell と同じ)
