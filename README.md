# Universal Asset Registry (UAR)

**「実体（データ）は一つ、解釈（レンダリング）は無限」**

> A physics-interaction engine for NFT assets.  
> Items ride, contain, move, affect territories and souls — across any world.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Protocol](https://img.shields.io/badge/Protocol-UAR%20v0.1-red)]()
[![Related: USL](https://img.shields.io/badge/Related-Universal--Soul--Logic-blue)](https://github.com/otspace0715/Universal-Soul-Logic)
[![Related: Map](https://img.shields.io/badge/Related-hex--shogun--map-green)](https://github.com/otspace0715/hex-shogun-map)

---

## 🌌 概要

UARは、NFTアセット（アイテム）を「特定のゲーム世界観」から解放し、複数のマルチバース（世界線）をまたいで共有・変容させるための**疎結合型アセット管理プロトコル**です。

サーバー側は「物理特性（数値）」と「歴史（魂）」のみを保持します。アイテムの名称・見た目・世界観への解釈は、すべてクライアント（ビューワー）側に委ねられます。

```
同一アセット: visual_hook_id = "TRANSPORT_ANIMAL_H01"

  戦国ビューワー    →  「早馬」        🐎
  ファンタジービューワー →  「ユニコーン」  🦄
  SF/サイバーパンクビューワー → 「ホバーバイク」 🏍️
```

---

## 🏗 三層プロトコルにおける位置づけ

本リポジトリは、以下の三層構造の**アセット層（物）**を担います。

| リポジトリ | 層 | 役割 |
|---|---|---|
| [Universal-Soul-Logic](https://github.com/otspace0715/Universal-Soul-Logic) | 魂（キャラクター） | NFTキャラクターのAP生成・成長・継承ロジック |
| [hex-shogun-map](https://github.com/otspace0715/hex-shogun-map) | 土地（領地） | ヘックスグリッドによる地形・領土管理エンジン |
| **Universal-Asset-Registry** | **物（アイテム）** | **NFTアセットの物理インタラクションエンジン** |

三層は互いに疎結合で連携します。アイテムは「魂」を乗せ、「領地」に影響を与え、他のアイテムと組み合わさってシナジーを生みます。

---

## 💡 コア・コンセプト：5つの物理インタラクション軸

UARのアイテムは、以下の5軸で定義された「物理的な振る舞い」を持ちます。

### 軸1：乗降・収納（コンテナ関係）

| パラメータ | 説明 |
|---|---|
| `rideable` | キャラクター（魂）が乗れるか |
| `capacity_souls` | 何人乗れるか |
| `exit_condition` | 降車条件（free / restricted / locked） |
| `container` | 他のアイテムを格納できるか |
| `item_slots` | 格納できるアイテム数 |
| `retrievable` | 格納物を取り出せるか |

### 軸2：可動性

| パラメータ | 説明 |
|---|---|
| `movable` | 移動可能か（false = 構造物・固定設置） |
| `autonomous` | 自律移動するか（例：早馬） |
| `move_speed` | 移動速度値 |

### 軸3：唯一性・数量

| パラメータ | 説明 |
|---|---|
| `unique` | 世界に一つしか存在しないか |
| `stackable` | 同種アイテムを複数まとめられるか |
| `stack_count` | 最大スタック数 |
| `consumable` | 使用すると消費されるか |
| `current_quantity` | 現在の残量 |

### 軸4：重量・物理制約

| パラメータ | 説明 |
|---|---|
| `weight` | アイテム自重（kg換算） |
| `weight_capacity` | 積載・格納できる最大重量 |
| `terrain_access` | 通行可能地形フラグ（hex-shogun-mapと連動） |

### 軸5：周囲への影響

アイテムは単独では完結せず、領地・キャラクター・他アイテムへの**バフ・デバフ・イベントトリガー**を持ちます。

```json
"effects": {
  "on_territory": [{ "type": "defense_value", "value": 30 }],
  "on_soul":      [{ "type": "attack_efficiency", "value": 1.4, "condition": "equipped" }],
  "on_item":      [{ "type": "fire_arrow_unlock", "target_id": "UAR-ITEM-010-GUNPOWDER" }]
}
```

---

## ⚡ シナジー：組み合わせが生む新しい価値

UARの醍醐味は、アイテム単体の性能ではなく、**同一領地内での組み合わせ**によって生まれる予期しない効果（シナジー）にあります。

| シナジー名 | 必要アイテム | 発動効果 |
|---|---|---|
| 🏯 不落城 | 城壁 ＋ 大筒 ＋ 狼煙台 | 「不落城」判定。外交カードとして機能 |
| 🔥 火矢射撃 | 大筒 ＋ 火薬 | 火攻め解放、攻城力+20 |
| 🛖 補給路 | 荷馬車 ＋ 米俵 | 兵糧補給自動化、スタミナ回復×1.5 |
| 🍵 茶会外交 | 茶碗（利休作） | 特定武将カードの訪問イベントトリガー |
| 🐎 騎馬陣形 | 馬 ×3以上 | 騎馬突撃解放 |
| 📡 早馬通信網 | 狼煙台 ＋ 早馬 | 情報伝達が国境を越えてリアルタイム化 |

---

## 📂 リポジトリ構造

```
/universal-asset-registry
├── index.html               # ブラウザデモ（シミュレーター）
├── /data
│   ├── item-samples.json    # コアアイテム定義（10種）
│   └── synergy-rules.json   # シナジールール定義（6種）
├── /viewer-skins
│   ├── sengoku-skin.json    # 戦国ビューワー解釈テーブル
│   ├── fantasy-skin.json    # ファンタジービューワー
│   └── scifi-skin.json      # SF/サイバーパンクビューワー
└── /schemas
    └── item.schema.json     # JSONスキーマ（バリデーション定義）※WIP
```

---

## 🧪 テストカバレッジ（10種コアアイテム）

| アイテム | 検証する軸 |
|---|---|
| 🐎 早馬 | 乗れる・自律移動・地形制限 |
| 🛖 荷馬車 | 乗せる・コンテナ・積載重量上限 |
| ⚔️ 名刀・へし切長谷部 | 唯一無二・耐久値・歴史値・キャラ影響 |
| 🌾 米俵 | スタック・消耗・領地食料影響 |
| 🏯 城壁 | 移動不可（構造物）・領地防御影響 |
| 💣 大筒 | 重量による移動コスト・攻城効果 |
| 🍵 茶碗（利休作） | 唯一・条件付きキャラ影響・歴史値 |
| 📦 木箱 | コンテナ・格納・取り出し |
| 🔥 狼煙台 | 移動不可・広域情報影響 |
| 🧨 火薬 | スタック消耗・組み合わせ効果依存 |

---

## 🚀 デモ（GitHub Pages）

`index.html` をブラウザで開くか、GitHub Pages経由でアクセスしてください。

デモで確認できること：

- **アイテム詳細** — 各アイテムの全パラメータ表示
- **確変（World Rendering）** — 同一アセットが3つの世界観でどう変わるか
- **積載シミュレーション** — 荷馬車に米俵を積む重量計算
- **地形判定** — 早馬が山岳・海上に進入したときのブロック検証
- **領地配置** — アイテムを領地に置いてシナジーをリアルタイム検出
- **テストログ** — 全操作の結果をコンソール形式で記録

---

## 🛠 クイックスタート

```bash
# 1. リポジトリをクローン
git clone https://github.com/otspace0715/universal-asset-registry.git
cd universal-asset-registry

# 2. ブラウザでデモを開く
open index.html
# または任意のローカルサーバーで
python -m http.server 8080
```

---

## 📐 アイテム定義の拡張

新しいアイテムを追加するには `data/item-samples.json` に以下の構造で追記します。

```json
{
  "asset_id": "UAR-ITEM-XXX-YOUR-ITEM",
  "asset_id_alias": "アイテム名（任意）",
  "asset_class": "001",
  "unique": false,
  "stackable": false,
  "weight": 0,
  "movable": true,
  "autonomous": false,
  "rideable": false,
  "capacity_souls": 0,
  "container": false,
  "item_slots": 0,
  "weight_capacity": 0,
  "consumable": false,
  "durability": 100,
  "history_count": 0,
  "terrain_access": ["all"],
  "visual_hook_id": "YOUR_HOOK_ID",
  "effects": {
    "on_territory": [],
    "on_soul": [],
    "on_item": []
  }
}
```

新しい世界観（スキン）を追加するには `viewer-skins/` に `your-skin.json` を作成し、`visual_hook_id` をキーとして解釈テーブルを定義してください。

---

## 🔗 連携プロトコル

| 連携先 | 連携方法 | 具体例 |
|---|---|---|
| Universal-Soul-Logic | `on_soul` effects / `capacity_souls` | キャラが馬に乗る、名刀を装備する |
| hex-shogun-map | `terrain_access` フラグ | 山岳ヘックスへの早馬進入判定 |

---

## 📜 ライセンス

MIT License — © 2025 otspace0715

---

## 🤝 コントリビュート

新しい `viewer-skin`（世界観）や `synergy-rules`（シナジー定義）のプルリクエストを歓迎します。
アイテム定義の拡張提案は Issue にてお気軽にどうぞ。
