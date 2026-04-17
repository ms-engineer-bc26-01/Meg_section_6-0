# Session6 技術スタック & コーディング規約

> ベースリポジトリ: [ms-engineer-bc26-01/Meg_section_6-0 / feature/express](https://github.com/ms-engineer-bc26-01/Meg_section_6-0/tree/feature/express)

---

## 技術スタック

| カテゴリ | 技術 | バージョン |
|---|---|---|
| 言語 | JavaScript (vanilla) | ECMAScript latest |
| 実行環境 | Node.js | — |
| リント | ESLint | ^10.1.0 |
| Git フック | Husky | ^9.1.7 |
| 段階的リント | lint-staged | ^16.4.0 |

> TypeScript・フレームワーク・DB・テストフレームワークは現時点では未導入（Section6 で追加予定）

---

## NPM スクリプト

```bash
npm run lint          # ESLint を全ファイルに実行
npm run lint:fix      # ESLint 自動修正
npm run lint:staged   # ステージング済みファイルのみリント（Husky から呼ばれる）
npm run prepare       # Husky 初期化（npm install 後に自動実行）
```

---

## ESLint ルール一覧

`eslint.config.js` に定義されているルールです。

| ルール | 設定 | 説明 |
|---|---|---|
| `indent` | 2 スペース | インデントは半角スペース 2 つ |
| `comma-spacing` | after: true | カンマの後にスペース必須 |
| `space-infix-ops` | error | 演算子の前後にスペース必須（例: `a + b`） |
| `camelcase` | error | 変数名・関数名は camelCase |
| `no-var` | error | `var` 禁止 → `const` / `let` を使用 |
| `brace-style` | 1tbs | 開き波括弧は同じ行に配置 |
| `no-alert` | error | `alert()` 禁止 |
| `id-match` | `^[a-z][a-zA-Z0-9]{0,11}$` | 変数名は小文字始まり、最大 12 文字 |

---

## コーディング規約（6-0 MUST.js より）

### 守るべき 7 つのルール

1. **インデントと改行を整える**
   - インデントは 2 スペース統一

2. **スペースを統一する**
   - カンマの後: スペース必須（`f(a, b)` ✅ / `f(a,b)` ❌）
   - 演算子の前後: スペース必須（`a + b` ✅ / `a+b` ❌）

3. **命名規則: camelCase**
   - ✅ `totalCount`, `myVariable`
   - ❌ `total_count`, `TotalCount`, `TOTALCOUNT`

4. **`var` 禁止 → `const` / `let` を使う**
   - 再代入なし → `const`
   - 再代入あり → `let`

5. **ブロック構造を整える**
   - 開き波括弧 `{` は同じ行に（1TBS スタイル）
   - `} else {` も同じ行に続ける

6. **`alert()` 禁止**
   - デバッグには `console.log()` を使用

7. **変数名は 12 文字以内**
   - ✅ `totalCount` (10 文字)
   - ❌ `totalNumberOfCatInTokyoAtLastYear` (長すぎ)

### 修正例

```javascript
// ❌ Before（規約違反）
var totalnumberOf_catIn_tokyo_atLastYear = 0;

function plus(n1,n2){return n1+n2;}

function f3(a, b){if(a==="test"&&b!=="temp")
{return b} else{return " " + b + "random text"}}

// ✅ After（規約準拠）
let totalCount = 0;

function plus(n1, n2) {
  return n1 + n2;
}

function f3(a, b) {
  if (a === "test" && b !== "temp") {
    return b;
  } else {
    return " " + b + "random text";
  }
}
```

---

## Git フック（Husky）

`.husky/pre-commit` によって、`git commit` 実行時に自動でリントが走ります。

```
git commit → pre-commit フック → npm run lint:staged → ESLint 自動修正 → コミット完了
```

lint-staged の設定:
```json
"lint-staged": {
  "*.js": ["eslint --fix"]
}
```

`.js` ファイルに対して `eslint --fix` が自動実行されます。

---

## 開発環境セットアップ

```bash
git clone https://github.com/ms-engineer-bc26-01/Meg_section_6-0.git
cd Meg_section_6-0
git checkout feature/express
npm install   # Husky も自動初期化される（prepare スクリプト）
```
