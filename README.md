# Non-overlapping Multi-Camera Tracking Dataset

## Overview

本データセットは、**非重複視野を有する複数カメラ環境における人物追跡（Non-overlapping Multi-Camera Tracking）**の研究を目的として構築したデータセットです。

このリポジトリでは、人物追跡アノテーションと、カメラ間で同一人物を対応付けるためのGlobal ID表を公開しています。撮影映像は個別に共有します。

---

## Camera Setup

本データセットの撮影では、対象空間に **Kenko KC-Z4K10** を8台設置し、各カメラの視野が互いに重複しないマルチカメラ環境を構築しました。

撮影条件は以下の通りです。

| Item                 | Setting            |
| -------------------- | ------------------ |
| Number of cameras    | 8                  |
| Camera               | Kenko KC-Z4K10     |
| Resolution           | 1920 × 1080 pixels |
| Frame rate           | 30 fps             |
| Camera configuration | Non-overlapping    |

撮影条件は1920 × 1080 pixels、30 fpsとし、高解像度・高フレームレートの高性能な撮影機器を前提としない設定としました。これにより、比較的一般的な撮影条件において、設置・運用コストを抑えたマルチカメラトラッキングの実現を想定しています。

---

## Data Collection

本データセットは、日常環境における自然発生的な人流をそのまま撮影したものではなく、**被験者にあらかじめ設定した動線を移動してもらうシナリオ撮影**によって構築しています。

撮影は以下の4日間に実施しました。

* July 20
* July 27
* July 30
* August 3

各撮影では通常歩行だけでなく、マルチカメラトラッキングにおいて多様な人物移動を評価できるよう、以下のような移動パターンを含めています。

* 通常歩行
* 複数人での移動
* 移動速度の変化
* 停止
* 引き返し
* その他の異なる移動経路

---

## Dataset Statistics

各撮影日に付与したGlobal ID数は以下の通りです。

| Sequence  | Number of Global IDs |
| --------- | -------------------: |
| 0720      |                   36 |
| 0727      |                   48 |
| 0730      |                   41 |
| 0803      |                   43 |
| **Total** |              **168** |

全シーケンスを合わせて、**168 Global IDs**を収録しています。

Global IDは各撮影シーケンス内で付与されており、異なるカメラに映った同一人物には同一のGlobal IDが対応します。

---

## Annotations

各カメラ映像には人物追跡用のBounding BoxおよびLocal Track IDを付与しています。

アノテーションはMOT形式を基本としており、各行は以下の情報から構成されます。

```text
frame_id, track_id, x, y, width, height, not_ignored, class_id, visibility
```

ここで、

* `frame_id`: フレーム番号
* `track_id`: カメラ内でのLocal Track ID
* `x, y`: Bounding Box左上座標
* `width, height`: Bounding Boxサイズ
* `not_ignored`: 評価対象かどうか
* `class_id`: クラスID
* `visibility`: 可視性

を表します。

Local Track IDには、研究で使用したCVATの表示ID（display ID）を使います。IDの有効範囲は撮影シーケンス内の各カメラです。

収録したGTは、研究時に使用したファイルと同じフレーム基準です。研究時と同じ同期済み動画で評価するときは、`0730/camC` のGTフレーム番号から10を引きます。[システム](https://github.com/shunemoto/non-overlap-mtmt-system)では既定設定で読み込み時に適用されます。

---

## Global ID

カメラをまたいだ同一人物の対応関係は、`global_id_map.csv` に記録しています。

基本形式は以下の通りです。 `local_track_id` は同じシーケンス・カメラの `gt.txt` の `track_id` に対応します。

```text
sequence_id,global_id,camera_id,local_track_id
```

---

## Repository Structure

```text
.
├── README.md
├── annotation/
│   ├── 0720/
│   ├── 0727/
│   ├── 0730/
│   └── 0803/
│
├── global_ids/
│   └── global_id_map.csv
│
└── figures/
    └── target_space.png
```

`annotation/` には各シーケンス・各カメラの人物追跡アノテーションを格納しています。

`global_ids/global_id_map.csv` には、各カメラのLocal Track IDとGlobal IDの対応関係を格納しています。

---

## Privacy

撮影映像は研究目的で収集したものです。

データセット公開にあたり、研究対象外の人物など、匿名化が必要な人物の顔にはぼかし処理を施しています。

撮影映像をお求めの方は、ご連絡ください。

---

## Intended Use

本データセットは、主に以下の研究用途を想定しています。

* Multi-Camera Tracking
* Multi-Target Multi-Camera Tracking (MTMC)
* Person Re-Identification
* Single-Camera Tracking
* Spatiotemporal person matching
* Camera topologyを利用した人物追跡

特に、カメラ間に非撮影領域が存在する**Non-overlapping Multi-Camera Tracking**の評価を目的としています。

---

## Citation

本データセットを研究成果等で利用する場合は、本データセットに関連する論文を引用してください。

論文情報は公開後に追記予定です。

```bibtex
@article{TBD,
  title   = {TBD},
  author  = {TBD},
  journal = {TBD},
  year    = {TBD}
}
```

---

## License

本データセットの利用条件については、本リポジトリに記載されたライセンスおよび利用規約を確認してください。

映像データ、アノテーションデータ、およびソースコードでは利用条件が異なる場合があります。
