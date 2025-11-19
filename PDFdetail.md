こちらは、提供されたPDFファイルに基づいたMarkdown形式のドキュメントです。

# Course DT2140: Laboration 2 - Sound and Gestures in Interaction (Autumn 2025)

[cite_start]このラボセッションでは、ラップトップとスマートフォンを使用して、身体の動きと音の間のつながりをマッピングします [cite: 5][cite_start]。スマートフォンの内蔵センサーを使用して、さまざまなジェスチャー動作を検出し区別し、提供された一連のサウンドモデルに意味のある方法で接続します [cite: 6]。

---

## 1. Groups and Equipment
* [cite_start]各ラボグループは最大2名で構成し、各グループは少なくとも1台のラップトップと1台のスマートフォンを持参する必要があります [cite: 8]。
* [cite_start]すべてのデバイス用のヘッドフォンも必ず持参してください [cite: 9]。
* [cite_start]機器の要件を満たすのが難しいグループは、適切な手配ができるよう、遅くともラボの1週間前までに担当教員に知らせる必要があります [cite: 9][cite_start]。これにはグループの再編成が含まれる可能性があります [cite: 10]。

---

## 2. Preparation
[cite_start]このドキュメントを読む以外に、ラボセッションが始まる前に行うべき必須の準備タスクがあります [cite: 12]：

* [cite_start]グループを結成し、STRAW POLLでラボセッションの時間を予約する [cite: 13]。
* [cite_start]FAUSTに慣れておく [cite: 14]。
* [cite_start]モバイルのセンサーに慣れておく [cite: 15]。
* [cite_start]Web Audioとモーションセンサーが携帯電話で動作するか確認するために、最初に例を試す [cite: 16]。
* [cite_start]GitHubの練習を始める [cite: 17]。
* [cite_start]ラボのセットアップと実行手順を注意深く読む [cite: 18]。
* [cite_start]Roberto Bresinの講義（Sound in Interaction）のノートを使用して、どのような動作と音のつながりが合理的か、そしてその理由は何かを考える [cite: 19][cite_start]。動作と音の良いつながり、または悪いつながりとは何かを検討してください [cite: 20]。
* [cite_start]（オプション）予定されたラボセッションの前に、できるだけ多くラボの作業を進めておく [cite: 21]。

> [cite_start]**注意:** 準備不足や適切な機器を持たずに現れたグループは、時間内にラボを合格することができず、追加の時間が必要になります [cite: 22]。

---

## 3. Lab Procedure
[cite_start]ラボは非常に簡単な導入から始まります [cite: 24][cite_start]。その後、準備中に生じた問題を議論する機会があり、グループでの作業を開始します [cite: 24]。

* [cite_start]**タスク:** ジェスチャー動作のセット（表1参照）と聴覚的結果のセット（表2参照）からペアを選択し、それらの間の接続を作成します [cite: 27]。
* [cite_start]**動作と結果:** 動作はすべて動きであり、結果はすべて異なるタイプの音です [cite: 28]。
* **要件:**
    * [cite_start]選択した動作が望ましい結果につながるようにマッピングを構築することが重要です [cite: 29]。
    * [cite_start]接続には動機付けが必要です（例：「ユースケースシナリオXを想定したため、YをZにマッピングしました...」） [cite: 30]。
    * [cite_start]デザイン決定について適切な動機付けを提示できた場合のみ、ラボアシスタントは接続を承認します [cite: 30]。
* [cite_start]**合格条件:** ラボに合格するには、少なくとも3つの承認された接続が必要です [cite: 31]。

### [cite_start]Table 1: Gestural Actions [cite: 36, 38]

| ACTIONS（動作） |
| :--- |
| The phone is shaken. (電話が振られる) |
| The phone is spun while lying on a flat surface. (平らな面で回転させられる) |
| The phone is used to point at the entrance. (入り口を指すために使われる) |
| The phone is used to point straight up. (真上を指すために使われる) |
| The phone is used to point towards north east. (北東を指すために使われる) |
| The phone is placed flat in the hand and tilted from side to side. (手のひらに平らに置き、左右に傾けられる) |
| The phone is held and used to detect all very small movements, but not large ones. (非常に小さな動きだけを検出し、大きな動きは検出しない) |
| The phone is held and used to detect all very large movements, but not small ones. (非常に大きな動きだけを検出し、小さな動きは検出しない) |
| The phone free falls for more than 30 cm. (30cm以上自由落下する) |
| The phone is used to measure the angle of a surface, like a level. (水準器のように、表面の角度を測るために使われる) |

### [cite_start]Table 2: Sounds synthesized using Faust [cite: 39, 40]

| SOUNDS（音） | |
| :--- | :--- |
| Bells | Rain |
| Bubbles | Sci-Fi (photon torpedo) |
| Creaking door | Thunder |
| Engine | Wind |
| Musical instruments | |

[cite_start]*(これらの音の多くは、Andy Farnellによって開発された例から引用されています [cite: 32])*

---

## 4. Useful Time Saving Hints!

### Check Your Hardware
[cite_start]ドキュメントの情報は通常正確ですが、デバイス、OSのバージョン、設定の特定の組み合わせにより、仕様とは異なるデータを受信する可能性があります [cite: 43][cite_start]。Web Audioとモーションセンサーが携帯電話で動作するか確認するために、最初に例を試してください [cite: 44]。

### Gyroscope Angles
[cite_start]ジャイロスコープのデータは、以下の図に示すように、ヨー（Yaw）、ピッチ（Pitch）、ロール（Roll）の形式です [cite: 46]。


* [cite_start]各軸のデータは $-180^{\circ}$ から $+180^{\circ}$ です [cite: 47]。
* [cite_start]これは、特定の回転位置を通過するとデータに不連続性やジャンプが生じることを意味します [cite: 47][cite_start]。パッチを設計する際はこれを考慮してください [cite: 48][cite_start]。馴染みがない場合はWikipediaを参照してください [cite: 48]。

---

## 5. Results
[cite_start]Canvasの「Assignment Lab 2: Sound and Gestures in Interaction」にアップロードするための短いテキストを準備し、以下の情報を記載してください [cite: 56]：

1.  [cite_start]このラボのパートナーのフルネーム [cite: 57]
2.  [cite_start]このラボの作業を保存したGitHubリポジトリへのリンク [cite: 58]
3.  [cite_start]以下の表のような構造を使用して作成した、ジェスチャー動作と音の間の接続 [cite: 59]。

[cite_start]*(4つ目のマッピングはオプションであり、時間があれば実装可能です [cite: 60])*

### [cite_start]Table 3: Suggested Mappings Template [cite: 61, 62]

| NUMBER | ACTION | SOUND |
| :--- | :--- | :--- |
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |

---

## 6. Examination
[cite_start]ラボアシスタントの1人に連絡し、Canvasにアップロードした作業を発表してください [cite: 64]。

---

*Would you like me to generate a checklist for the preparatory tasks mentioned in the document?*