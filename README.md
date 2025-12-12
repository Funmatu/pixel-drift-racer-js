# Pixel Drift Racer JS 🏎️💨

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-JavaScript-yellow.svg)
![Engine](https://img.shields.io/badge/engine-No%20Libraries%20(Vanilla)-green.svg)

**外部ライブラリ不使用。HTML5 CanvasとVanilla JavaScriptのみで構築された、物理演算ベースの2Dトップダウン・レーシングゲーム。**

---

## 🎮 デモプレイ (Live Demo)

ブラウザで今すぐプレイできます（インストール不要）:
### [👉 Click here to Play (GitHub Pages)](https://funmatu.github.io/pixel-drift-racer-js/)

---

## 📖 概要 (Overview)

**Pixel Drift Racer JS** は、レトロなドット絵スタイルと、本格的なドリフト挙動を組み合わせたタイムアタックレースゲームです。
PhaserやThree.jsなどのゲームエンジンを使用せず、ゲームループ、衝突判定、物理演算、レンダリングパイプラインをすべて自前のJavaScriptで実装しています。

### 主な機能
* **物理演算エンジン:** 慣性、摩擦、加速度、遠心力を計算し、独特の「ドリフト」感覚を実現。
* **路面判定:** 舗装路（高グリップ）と芝生（低グリップ）で挙動がリアルタイムに変化。
* **スキッドマーク（タイヤ痕）:** オフスクリーンCanvasを使用した描画最適化により、パフォーマンスを落とさずに走行軌跡を描画。
* **ラップタイム計測:** チェックポイントシステムによる不正防止機能付きの正確なラップ計測、ベストタイム記録。
* **レスポンシブ:** ウィンドウサイズに合わせてアスペクト比を維持したままスケーリング。

---

## 🕹️ 操作方法 (Controls)

PCキーボードで操作します。一度画面をクリックしてフォーカスを当ててから操作してください。

| キー | 動作 |
| :--- | :--- |
| **W / ↑** | アクセル (加速) |
| **S / ↓** | ブレーキ / バック |
| **A / ←** | 左旋回 |
| **D / →** | 右旋回 |
| **SPACE** | **ハンドブレーキ (急旋回・ドリフト誘発)** |
| **R** | リセット (スタート位置に戻る) |

---

## 🛠️ 技術的な解説 (Technical Details)

このプロジェクトは、ゲーム開発の基礎学習や、軽量なWebゲームのプロトタイプとして設計されています。

### 1. ゲームループ (Game Loop)
`requestAnimationFrame` を使用しつつ、物理演算の更新頻度を固定（Fixed Timestep）しています。
```javascript
const FPS = 60;
const DT = 1 / FPS;
// 描画フレームレートに関わらず、物理挙動は常に1/60秒単位で計算され、一貫性を保ちます。
````

### 2\. ドリフト物理 (Drifting Physics)

単純な方向転換ではなく、車の「向き（Heading）」と「進行方向（Velocity Vector）」を分け、それらを線形補間（Lerp）することでドリフトを表現しています。

  * **グリップ走行:** 進行方向が素早く向きに追従。
  * **ドリフト走行:** 進行方向が向きに追従するのに遅れが生じ、横滑りが発生。
  * **ハンドブレーキ:** 補間係数（`DRIFT_FACTOR`）を動的に下げることで、意図的に滑りやすくしています。

### 3\. レンダリング最適化 (Rendering Optimization)

  * **メインCanvas:** 車、マップ、HUDを毎フレーム描画。
  * **スキッドCanvas (Off-screen):** タイヤ痕専用のバッファ。毎フレームクリアせず、タイヤ痕を「描き足し」ていきます。メインCanvasには画像として転写するだけなので、痕跡が増えても描画コストが増えません。

-----

## 📂 インストールと実行 (Local Setup)

このゲームは単一のHTMLファイルで完結しているため、特別なビルドツールは不要です。

1.  **リポジトリをクローン:**
    ```bash
    git clone https://github.com/Funmatu/pixel-drift-racer-js.git
    ```
2.  **実行:**
    フォルダ内の `index.html` をお使いのWebブラウザ（Chrome, Firefox, Edgeなど）で開くだけで実行可能です。

-----

## ⚙️ カスタマイズ (Customization)

ソースコード内の定数を変更することで、ゲームバランスを調整できます。

  * **車の挙動:** `const PHYS = { ... }` ブロック内の数値を変更。
      * `ACCEL`: 加速度
      * `DRIFT_FACTOR`: 滑りやすさ（1.0に近いほどグリップ、低いほど氷の上）
  * **コース:** `const mapLayout = [ ... ]` 配列を編集。
      * `0`: 芝生, `1`: 道路, `3`: 壁

-----

## 📝 ライセンス (License)

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.
