# BitzCode

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzCodeは、BitzLabsエコシステム内で、C#, JavaScript, Pythonなどのプログラミング言語のソースコードを構造的に扱うための専門ライブラリです。

## 主な機能

-   **`ProcAST`の定義**: 主要なプログラミング言語の構文（変数宣言、関数呼び出し、制御構文など）を抽象的に表現するAST。ESTree仕様に強くインスパイアされています。
-   **パーサー**: 各種プログラミング言語のソースコードを解釈し、`ProcAST`を生成します。（初期実装ではRoslyn, Acornなどの既存ライブラリのラッパーとして構築）
-   **2つの主要アプリケーション基盤 (第3目標)**:
    1.  **フォーマッタ**: `ProcAST`を整形済みコード文字列に変換するための基盤を提供します。
    2.  **リンター**: `ProcAST`を走査し、コーディングルール違反を検出するためのプラグイン可能なルールエンジンを提供します。
-   **シンタックスハイライト用コンバーター**: `ProcAST`を、シンタックスハイライト用のHTML(`<span>`タグとCSSクラス)に変換します。

## このライブラリの位置づけ

`BitzCode`は、BitzLabsの**第3目標**である「コード解析と変換」の中核を担います。また、第1目標においても、`BitzDoc`と連携してドキュメント内のコードブロックにシンタックスハイライトを適用する機能を提供します。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`

## **✅ 初期開発ToDoリスト**

1.  **`ProcAST`の型定義**:
    *   `IProcNode` インターフェースを作成（`IAstNode`を継承）。
    *   **文 (Statement) ノード**: `VariableDeclaration`, `IfStatement`, `ForStatement`, `ReturnStatement` などのクラスを定義。
    *   **式 (Expression) ノード**: `BinaryExpression` (`left`, `right`, `operator`), `CallExpression` (`callee`, `arguments`), `Identifier`, `Literal` などのクラスを定義。
    *   **宣言 (Declaration) ノード**: `FunctionDeclaration`, `ClassDeclaration` などのクラスを定義。
    *   *ヒント: まずはJavaScriptのESTree仕様を参考に、最も基本的なノードから定義していくのが良いでしょう。*

2.  **パーサーのインターフェース**:
    *   `ICodeParser` インターフェースを定義。`public IProcNode Parse(string code, string language)` のようなメソッドを持つ。
    *   初期実装として、`JavaScriptParser` クラスを作成。内部でC#から呼び出せるJSパーサーライブラリ（例: `Jint`上で`Acorn`を動かす、またはWASM化されたパーサーを利用）をラップする形で実装の骨格を作る。

3.  **シンタックスハイライト用コンバーターの骨格**:
    *   `SyntaxHighlightConverter` クラスを作成。
    *   `public string ToHtml(IProcNode procNode)` のようなメソッドを定義。
    *   `ProcAST`をVisitorパターンで走査し、ノードの種類に応じてCSSクラスを付与した`<span>`タグを生成するロジックを実装する。
