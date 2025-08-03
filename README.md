# PicoRin（ピコリン）

PicoRin は、Raspberry Pi Pico 向けに設計された、状態制御ベースの産業用制御DSL（ドメイン固有言語）開発環境です。YAML形式でステートマシンの構成を記述し、WebベースのUIで直感的に編集・ビルド・デバッグが可能です。

## 特徴

- 🧠 **状態遷移ベースの明確な制御構造**：ラダー言語の自己保持や条件分岐の煩雑さを解消。
- 📄 **YAMLによるDSL記述**：誰でもわかりやすく読み書きでき、グラフィカルエディタにも対応予定。
- ⚙️ **Raspberry Pi Pico 向け C++ コード生成**：pico-sdk を使ったネイティブビルド。
- 🔧 **信号演算と関数実行の分離**：`when` 句に論理条件、`do` 句に処理を明確に記述。
- 🧩 **動的型解釈**：16ビット信号を用途に応じて整数/ビット列/フラグとして扱える。
- 🧪 **デバッグ対応**：2コア構成を活かし、片方のコアで仮想COM通信により変数・ステートを定期送信。
- 🌐 **FastAPI + Web UI**：デバイスフリーな開発環境を提供し、日本語にも完全対応。
- 📦 **外部デバイス定義読み込み**：Pico固有のC++ヘッダや関数を拡張としてDSLに統合可能。

## サンプル YAML

```yaml
cycle: 10ms

states:
  - name: idle
    when: booted
    do: led_off()

  - name: ready
    when: up(start_btn) and safe
    do: led_blink()

  - name: running
    when: up(run_btn)
    do: motor_on()

  - name: stop
    when: not safe
    kind: force
    do: motor_off()

vars:
  safe: input_pin[3]
  start_btn: input_pin[4]
  run_btn: input_pin[5]
```

## ビルド手順

1. Web UIでYAMLファイルを作成・保存
2. [ビルド] をクリックして `.uf2` ファイルをダウンロード
3. Pico を USBマスストレージモードにしてファイルを書き込み

## デバッグ

- USB仮想COM経由で状態と変数を JSON or バイナリ形式で定期送信
- Web UIからステートと変数の状況をリアルタイム表示
- 任意の変数を書き換えて即座に動作確認も可能

## 開発・拡張

- 外部デバイス（センサ・出力）に応じた C++ ヘッダ・YAML を読み込み
- 拡張関数を `do` に記述して Pico に反映可能

## 日本語対応

- UI、エラーメッセージ、関数名、ラベルなど全て日本語表示に対応

## ライセンス

MIT License

---

© 2025 PicoRin Project
