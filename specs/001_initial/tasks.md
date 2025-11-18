# zsh-necromancer 詳細作業計画

## 全体概要
TypeScript/Deno製のzshプラグインマネージャー「zsh-necromancer」の実装
**総タスク数: 63タスク**

---

## 1. プロジェクト基本構造のセットアップ (3タスク)

- [ ] deno.jsonの作成
- [ ] srcディレクトリ構造の作成
- [ ] .gitignoreファイルの作成

---

## 2. 型定義ファイルの作成 (4タスク)

- [ ] types.ts - Plugin型の定義
- [ ] types.ts - IceModifiers型の定義
- [ ] types.ts - Config型の定義
- [ ] types.ts - LockFile型の定義

---

## 3. 設定ファイル管理モジュール (4タスク)

- [ ] config.ts - 設定ファイルパスの解決処理
- [ ] config.ts - JSON読み込み処理
- [ ] config.ts - 設定バリデーション（atフィールド必須チェック）
- [ ] config.ts - SHA形式検証（7文字/40文字）

---

## 4. ロックファイル管理モジュール (4タスク)

- [ ] lock.ts - ロックファイルパスの解決処理
- [ ] lock.ts - ロックファイル読み込み処理
- [ ] lock.ts - ロックファイル書き込み処理
- [ ] lock.ts - ロックファイル更新処理

---

## 5. Git操作モジュール (5タスク)

- [ ] git.ts - git cloneコマンドのラッパー実装
- [ ] git.ts - git fetchコマンドのラッパー実装
- [ ] git.ts - git checkoutコマンドのラッパー実装
- [ ] git.ts - 現在のコミットSHA取得処理
- [ ] git.ts - エラーハンドリング（即座に失敗）

---

## 6. プラグイン管理コアロジック (6タスク)

- [ ] plugin.ts - プラグインディレクトリパス解決
- [ ] plugin.ts - プラグインタイプ判定ロジック（command/snippet/plugin）
- [ ] plugin.ts - plugin型のファイル自動検出（優先順位付き）
- [ ] plugin.ts - pick修飾子の処理
- [ ] plugin.ts - hookBuildコマンドの実行処理
- [ ] plugin.ts - atload/atinit修飾子の処理

---

## 7. installコマンドの実装 (5タスク)

- [ ] commands/install.ts - 設定ファイルの読み込み
- [ ] commands/install.ts - シーケンシャルなプラグイン処理ループ
- [ ] commands/install.ts - 各プラグインのクローン処理
- [ ] commands/install.ts - コミットSHAへのチェックアウト
- [ ] commands/install.ts - hookBuildの実行
- [ ] commands/install.ts - ロックファイルの更新

---

## 8. updateコマンドの実装 (4タスク)

- [ ] commands/update.ts - 引数解析（全体/個別プラグイン）
- [ ] commands/update.ts - 最新コミット取得処理
- [ ] commands/update.ts - git fetchとcheckout処理
- [ ] commands/update.ts - ロックファイルの更新

---

## 9. loadコマンドの実装 (5タスク)

- [ ] commands/load.ts - ロックファイルの読み込み
- [ ] commands/load.ts - プラグインタイプ別のスクリプト生成
- [ ] commands/load.ts - PATH追加処理（command型）
- [ ] commands/load.ts - source文の生成（plugin/snippet型）
- [ ] commands/load.ts - atinit/atloadコマンドの埋め込み

---

## 10. listコマンドの実装 (2タスク)

- [ ] commands/list.ts - 設定ファイルの読み込み
- [ ] commands/list.ts - プラグイン一覧の整形表示

---

## 11. statusコマンドの実装 (3タスク)

- [ ] commands/status.ts - 設定ファイルとロックファイルの読み込み
- [ ] commands/status.ts - インストール済みプラグインの現在のSHA取得
- [ ] commands/status.ts - 設定値との比較と差分表示

---

## 12. cleanコマンドの実装 (4タスク)

- [ ] commands/clean.ts - 設定ファイルの読み込み
- [ ] commands/clean.ts - プラグインディレクトリのスキャン
- [ ] commands/clean.ts - 未設定プラグインの特定
- [ ] commands/clean.ts - 確認なしでの削除処理

---

## 13. メインCLIエントリーポイント (4タスク)

- [ ] main.ts - CLIフレームワークのセットアップ（Deno標準ライブラリ使用）
- [ ] main.ts - コマンドルーティングの実装
- [ ] main.ts - ヘルプメッセージの実装
- [ ] main.ts - エラーハンドリングとexit code管理

---

## 14. README.mdの作成 (5タスク)

- [ ] README.md - プロジェクト概要の記述
- [ ] README.md - インストール方法の記述
- [ ] README.md - 設定ファイルの例と説明
- [ ] README.md - 各コマンドの使用方法
- [ ] README.md - Ice modifiersの詳細説明

---

## 15. テストと最終確認 (3タスク)

- [ ] 基本機能のテスト実行
- [ ] エラーハンドリングの動作確認
- [ ] ドキュメントの最終レビュー

---

## 重要な実装ポイント

### 必須要件
- ✅ `at`フィールド（コミットSHA）は必須 - 欠落時は即座に終了
- ✅ エラー発生時は即座に失敗（clone失敗、ビルド失敗など）
- ✅ プラグイン処理はシーケンシャル（並列処理なし）

### ファイルパス
- 設定: `~/.config/zsh-necromancer/config.json`
- ロック: `~/.config/zsh-necromancer/zsh-necromancer.lock.json`
- プラグイン: `~/.config/zsh-necromancer/plugins`

### サポート機能
- SHA形式: 7文字（短縮）/ 40文字（完全）
- プラグインタイプ: `command`, `snippet`, `plugin`（デフォルト）
- Ice modifiers: `name`, `at`, `as`, `pick`, `hookBuild`, `atload`, `atinit`
