# BitzCode

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzCodeは、BitzLabsエコシステム内で、C#, JavaScript, Pythonなどのプログラミング言語のソースコードを構造的に扱うための専門ライブラリです。

## 主な機能

-   **`ProcAST`の定義**: 主要なプログラミング言語の構文（変数宣言、関数呼び出し、制御構文など）を抽象的に表現するAST。ESTree仕様に強くインスパイアされています。
-   **パーサー**: 各種プログラミング言語のソースコードを解釈し、`ProcAST`を生成します。（初期実装ではRoslyn, Acornなどの既存ライブラリのラッパーとして構築）
-   **シンタックスハイライト用コンバーター**: `ProcAST`を、シンタックスハイライト用のHTML(`<span>`タグとCSSクラス)に変換します。
-   **フォーマッタ基盤 (第3目標)**: `ProcAST`を整形済みコード文字列に変換するための基盤を提供します。
-   **リンター基盤 (第3目標)**: `ProcAST`を走査し、コーディングルール違反を検出するためのプラグイン可能なルールエンジンを提供します。

## ✅ 初期開発ToDoリスト

1.  **`ProcAST`の型定義**:
    *   `IProcNode`インターフェース(`IAstNode`継承)を定義。
    *   `VariableDeclaration`, `IfStatement`, `BinaryExpression`, `CallExpression`など、ESTreeを参考に主要なノードクラスの「殻」を作成。
2.  **パーサーのインターフェース**:
    *   `ICodeParser`インターフェースを定義。`Parse(string code, string language)`メソッドを持つ。
    *   既存のJSパーサーライブラリ等をラップする形で、`JavaScriptParser`の骨格を実装。
3.  **シンタックスハイライト用コンバーター**:
    *   `SyntaxHighlightConverter`クラスを作成。`ToHtml(IProcNode procNode)`メソッドを持つ。
    *   Visitorパターンで`ProcAST`を走査し、ノードの種類に応じてCSSクラス付きの`<span>`タグを生成するロジックの骨格を実装。

## このライブラリの位置づけ

`BitzCode`は、BitzLabsの**第3目標**である「コード解析と変換」の中核を担います。また、第1目標においても、`BitzDoc`と連携してドキュメント内のコードブロックにシンタックスハイライトを適用する機能を提供します。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
