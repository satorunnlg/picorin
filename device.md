# デバイス拡張定義ファイル - device.md

このドキュメントでは、PicoRinで特定のデバイスに対応した**変数や関数をDSLで使用可能にする**方法を説明します。

---

## 📦 デバイス拡張の目的

PicoRinでは、Raspberry Pi Pico固有のピンやセンサー、外部モジュール（例：LED、リレー、ステッピングモーター）に対し、YAML DSLで扱える**カスタム関数と変数**を定義できます。

これにより、汎用的な状態遷移ロジックに対して、**デバイス特化のロジックを疎結合で統合**できます。

---

## 📁 拡張ファイル構成例

```
devices/
└── motor_driver/
    ├── motor_driver.h
    ├── motor_driver.cpp
    └── device.yaml
```

---

## 🔧 device.yaml の書き方

```yaml
name: motor_driver
description: DCモーター制御用ドライバ関数群
functions:
  - name: set_motor_speed
    description: モーターの速度を設定する
    args:
      - name: speed
        type: int
    returns: none
  - name: stop_motor
    description: モーターを停止する
    args: []
    returns: none

variables:
  - name: motor_enabled
    type: bit
    description: モーター動作状態（1:ON, 0:OFF）
  - name: motor_speed
    type: int
    description: 現在のモーター速度（0-255）
```

---

## 💡 使用例（DSL YAML内）

```yaml
states:
  - name: idle
    when: not motor_enabled
    do:
      - stop_motor

  - name: running
    when: up start_button and motor_enabled
    do:
      - set_motor_speed(180)
```

---

## 🧠 開発時のポイント

- `functions` の引数型は `bit`, `int`, `float`, `string` に対応。
- `variables` は型と説明を記述すれば、内部的にアクセス可能になります。
- device.yaml に対応する `.cpp` / `.h` を開発者が用意し、パーサが自動で呼び出せるようコード生成します。

---

## 📤 拡張ファイルのUI取り込み

ブラウザUI上で以下を可能にします：

- `device.yaml` 読み込み → 変数・関数の補完表示
- `motor_driver.h/cpp` をプロジェクトに追加
- 未定義関数や変数使用時のリアルタイムエラー通知

---

## 📚 今後の拡張予定

- SPI/I2C接続デバイスのテンプレート定義
- YAMLからピン割り当てのGUI補助
- Pico SDKと自動リンクするヘッダ検出機能

---

## 📝 備考

- 同一プロジェクトに複数のデバイス定義を追加可能
- 名前空間や関数衝突に注意（コンフリクト検出あり）

