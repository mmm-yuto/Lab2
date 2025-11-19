# Lab 2 実装内容まとめ

## 1. システム概要

スマートフォンの内蔵センサーを使用してジェスチャー動作を検出し、FAUSTで合成された音をWeb Audio APIで再生するインタラクティブシステムを構築します。

## 2. 技術スタック

### 必須技術
- **Web Audio API**: 音の再生と制御
- **DeviceOrientationEvent / DeviceMotionEvent**: スマートフォンのセンサーデータ取得
  - ジャイロスコープ（Yaw, Pitch, Roll: -180°〜+180°）
  - 加速度計（Accelerometer）
  - 磁力計（Magnetometer/Compass）
- **FAUST**: 音の合成（Bells, Rain, Bubbles, Sci-Fi, Thunder, Wind, Engine, Creaking door, Musical instruments）
- **HTML/CSS/JavaScript**: フロントエンド実装
- **GitHub**: コード管理と提出

### 開発環境
- ラップトップ（開発用）
- スマートフォン（テスト用、HTTPS環境が必要）
- ヘッドフォン

## 3. 実装する機能

### 3.1 センサーデータ取得モジュール

#### 実装内容
- デバイスの向き・動きのイベントリスナー設定
- センサーデータの正規化とフィルタリング
- ジェスチャー検出のためのデータ処理

#### 必要なAPI
```javascript
// DeviceOrientationEvent (ジャイロスコープ)
window.addEventListener('deviceorientation', (event) => {
  const alpha = event.alpha; // Z軸回転 (Yaw) -180°〜+180°
  const beta = event.beta;   // X軸回転 (Pitch) -180°〜+180°
  const gamma = event.gamma; // Y軸回転 (Roll) -180°〜+180°
});

// DeviceMotionEvent (加速度計)
window.addEventListener('devicemotion', (event) => {
  const accel = event.acceleration;
  const accelGravity = event.accelerationIncludingGravity;
});
```

### 3.2 ジェスチャー検出モジュール

#### 実装するジェスチャー（表1から少なくとも3つ選択）

1. **電話が振られる (Shake)**
   - 検出方法: 加速度の変化率が閾値を超える
   - 実装: 加速度の差分を計算し、一定以上の変化を検出

2. **平らな面で回転 (Spin on surface)**
   - 検出方法: ジャイロスコープのYaw値の連続的な変化
   - 実装: Yaw値の変化率を監視

3. **入り口を指す (Point at entrance)**
   - 検出方法: コンパス（磁力計）と加速度計の組み合わせ
   - 実装: デバイスの向きと方位角を計算

4. **真上を指す (Point straight up)**
   - 検出方法: Pitch ≈ 90° または Beta ≈ 90°
   - 実装: ジャイロスコープのBeta値を監視

5. **北東を指す (Point towards north east)**
   - 検出方法: コンパスとジャイロスコープの組み合わせ
   - 実装: 方位角が45°付近であることを検出

6. **左右に傾ける (Tilt side to side)**
   - 検出方法: Roll (Gamma) 値の変化
   - 実装: Gamma値が一定範囲を超えることを検出

7. **小さな動きのみ検出 (Small movements only)**
   - 検出方法: 加速度の変化が小さい閾値以下
   - 実装: ローパスフィルターと閾値判定

8. **大きな動きのみ検出 (Large movements only)**
   - 検出方法: 加速度の変化が大きい閾値以上
   - 実装: ハイパスフィルターと閾値判定

9. **自由落下 (Free fall > 30cm)**
   - 検出方法: 加速度が重力加速度に近づく（約0G）
   - 実装: 加速度の総和が閾値以下になることを検出

10. **水準器として使用 (Level measurement)**
    - 検出方法: PitchとRollの値から角度を計算
    - 実装: BetaとGammaから表面の角度を算出

### 3.3 音の合成・再生モジュール

#### FAUST音源の統合
- FAUSTで生成された音源ファイル（.dsp）をWeb Audio APIで読み込み
- AudioWorkletまたはScriptProcessorNodeを使用してリアルタイム処理
- パラメータ制御（音量、周波数、エンベロープなど）

#### 実装する音（表2から選択）
- Bells
- Rain
- Bubbles
- Sci-Fi (photon torpedo)
- Thunder
- Wind
- Engine
- Creaking door
- Musical instruments

### 3.4 マッピング・接続モジュール

#### 実装内容
- ジェスチャー検出イベントと音の再生を接続
- パラメータマッピング（ジェスチャーの強度→音量、角度→周波数など）
- 状態管理（現在再生中の音、検出中のジェスチャー）

#### マッピング例
```javascript
// 例: 振る動作 → Thunder（雷）
function mapShakeToThunder(shakeIntensity) {
  const volume = mapIntensityToVolume(shakeIntensity);
  const frequency = mapIntensityToFrequency(shakeIntensity);
  playThunderSound(volume, frequency);
}
```

## 4. ファイル構造

```
KTH_Lab2/
├── index.html              # メインHTMLファイル
├── css/
│   └── style.css          # スタイリング
├── js/
│   ├── sensor.js          # センサーデータ取得
│   ├── gesture.js         # ジェスチャー検出
│   ├── audio.js           # 音の再生・制御
│   ├── mapping.js         # ジェスチャーと音のマッピング
│   └── main.js            # メインロジック
├── faust/
│   ├── bells.dsp          # FAUST音源ファイル
│   ├── rain.dsp
│   ├── thunder.dsp
│   └── ...                # その他の音源
├── sounds/
│   └── ...                # コンパイル済み音源（.wasm等）
├── README.md              # プロジェクト説明
└── implement.md           # このファイル
```

## 5. 実装手順

### Phase 1: 環境セットアップ
1. [ ] GitHubリポジトリを作成
2. [ ] 基本的なHTMLファイルを作成
3. [ ] HTTPS環境をセットアップ（ローカル開発サーバー）
4. [ ] デバイスセンサーのアクセス許可を実装

### Phase 2: センサーデータ取得
1. [ ] DeviceOrientationEventの実装
2. [ ] DeviceMotionEventの実装
3. [ ] センサーデータの可視化（デバッグ用）
4. [ ] データの正規化とフィルタリング

### Phase 3: ジェスチャー検出
1. [ ] 最初のジェスチャーを実装（例: Shake）
2. [ ] 2つ目のジェスチャーを実装（例: Tilt）
3. [ ] 3つ目のジェスチャーを実装（例: Point up）
4. [ ] 各ジェスチャーの検出精度を調整

### Phase 4: 音の統合
1. [ ] FAUST音源の準備・コンパイル
2. [ ] Web Audio APIでの音源読み込み
3. [ ] 音の再生機能の実装
4. [ ] パラメータ制御の実装

### Phase 5: マッピング実装
1. [ ] マッピング1: ジェスチャー1 → 音1（動機付けを明確に）
2. [ ] マッピング2: ジェスチャー2 → 音2（動機付けを明確に）
3. [ ] マッピング3: ジェスチャー3 → 音3（動機付けを明確に）
4. [ ] （オプション）マッピング4の実装

### Phase 6: テストと調整
1. [ ] 各マッピングの動作確認
2. [ ] センサーデータの不連続性への対応（ジャイロスコープの-180°/+180°ジャンプ）
3. [ ] パフォーマンス最適化
4. [ ] 異なるデバイスでのテスト

### Phase 7: ドキュメント作成
1. [ ] README.mdの作成
2. [ ] マッピング表の作成（表3の形式）
3. [ ] 動機付けの説明文を作成
4. [ ] GitHubリポジトリの整理

## 6. 実装の注意点

### 6.1 センサーデータの処理
- **ジャイロスコープの不連続性**: -180°と+180°の境界でジャンプが発生するため、角度の差分計算時に注意
- **デバイス間の差異**: 異なるデバイスでセンサーデータの範囲や精度が異なる可能性
- **サンプリングレート**: イベントの発生頻度はデバイス依存

### 6.2 Web Audio API
- **HTTPS必須**: モバイルデバイスのセンサーAPIはHTTPS環境でないと動作しない
- **ユーザーインタラクション**: 音の再生にはユーザーのインタラクション（クリック等）が必要
- **パフォーマンス**: AudioWorkletの使用を推奨（ScriptProcessorNodeは非推奨）

### 6.3 マッピングの設計
- **意味のある接続**: ジェスチャーと音の接続は直感的で意味のあるものにする
- **動機付け**: 各マッピングには明確なユースケースシナリオと動機付けが必要
- **パラメータマッピング**: ジェスチャーの強度や角度を音のパラメータにマッピングすることで、よりインタラクティブに

## 7. 推奨マッピング例（参考）

### マッピング1: Shake → Thunder
- **動機付け**: 雷が鳴る瞬間の衝撃的な動きを、電話を振る動作で表現。振る強さに応じて雷の音量と強度が変化。
- **実装**: 加速度の変化率を検出し、Thunder音の音量とエンベロープにマッピング

### マッピング2: Tilt side to side → Wind
- **動機付け**: 風が吹く方向の変化を、デバイスを左右に傾ける動作で表現。傾きの角度に応じて風の強さと方向が変化。
- **実装**: Roll (Gamma) 値をWind音の周波数と音量にマッピング

### マッピング3: Point straight up → Bells
- **動機付け**: 鐘を鳴らす動作（上に向かって引っ張る）を、デバイスを真上に向ける動作で表現。正確に真上を向いた時に鐘が鳴る。
- **実装**: Pitch (Beta) ≈ 90° を検出し、Bells音を再生

## 8. チェックリスト

### 実装完了チェック
- [ ] 少なくとも3つのジェスチャー検出が実装されている
- [ ] 少なくとも3つの音が実装されている
- [ ] 少なくとも3つのマッピングが実装されている
- [ ] 各マッピングに明確な動機付けがある
- [ ] スマートフォンで動作確認が完了している
- [ ] GitHubリポジトリにコードがアップロードされている

### 提出物チェック
- [ ] パートナーのフルネーム
- [ ] GitHubリポジトリへのリンク
- [ ] マッピング表（表3の形式）が完成している
- [ ] 各マッピングの動機付けが文書化されている

