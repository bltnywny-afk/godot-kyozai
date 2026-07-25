# ノードの木 — Godot 4 学習教材サイト

Godot 4 でのゲームづくりを独学するための静的サイト。ビルド工程なし、
フレームワークなし。素の HTML/CSS/JS のみ。GitHub Pages で公開する。

## 構成

```
index.html      ダッシュボード（進捗・一覧・同期・配色切替）
<id>.html       各回のページ（43枚）。id は s0〜s20, s7a, s7b, g1, g2,
                t1〜t7（第5部）, p0〜p9（実践）, ref（付録）
assets/style.css  全ページ共通。CSS変数でダーク（既定）/ライト
assets/app.js     全ロジック。先頭に LESSONS マニフェスト（JSON）
```

## 絶対に守ること

1. **回番号はハードコードしない。** 見出しの番号は `<b class="lno"></b>`、
   本文中の参照は `<a class="xref" data-ref="対象id" href="対象id.html">第N回</a>`
   の形で書く。表示される番号は app.js が LESSONS の並び順から計算して埋める。
   静的な「第N回」という文字列を本文に直接書かない。

2. **ページを追加・削除・並べ替えしたら、app.js 先頭の LESSONS も必ず同期する。**
   各要素: `{id, title, num, part, kind, steps, file}`
   - kind: "lesson"（読む回）| "build"（実践 B1〜B6）| "sketch"（設計メモ B7〜B9）
     | "other"（p0, ref）
   - num は仮でよい（表示は再計算される）が、lesson は "01" 形式、build/sketch は
     "B1" 形式で通し番号を振り直すこと
   - steps: そのページ内の `<h3 class="st">` の個数と一致させる
   - file: その章の既定スクリプト名（例 "main.gd"）。無ければ ""

3. **進捗データの互換を壊さない。**
   - localStorage キー: `godot-kyozai-v1`、中身 `{done:{}, steps:{}, theme}`
   - steps のキーは `"<ページid>#<ステップ番号(0始まり)>"`
   - 同期コード: `KYOZAI1.` + base64(JSON `{v:1, o:"010...", s:[...]}`)。
     `o` のビット順 = LESSONS の並びのうち kind が lesson/build のもの。
     **並びの途中に回を挿入するとビット位置がずれ、既存の同期コードが
     読めなくなる。** 挿入が必要なときはユーザーに確認してから行う。

## ページの書き方（新しい回を作るとき）

- 既存の s1.html などを複製して作るのが安全。`<body data-page="lesson" data-id="新id">` を忘れない
- 本文は `<section id="新id" data-title="短い題名">` で包む
- 実践章は `data-kind="build"`、設計メモは更に `data-sketch="1"`
- 見出し構造: `.eyebrow`（部名 + lno + 時間）→ h2 → `.aim`
- コード片は `<pre><code>` 。先頭コメント `# ファイル名.gd` を書くと
  コードカードにファイル名が表示される（app.js が自動判定）
- ステップは `<span class="stepnum">STEP n</span>` + `<h3 class="st">見出し</h3>`。
  完了ボタンは app.js が自動挿入する
- 確認ポイントは `.check`、注意は `.note`、課題は `.try`、
  仕様は `.spec`、設計メモ注意書きは `.sketch`
- ノード構成図の pre（├─ を含む）はコードカード化されず本文に残る（意図どおり）

## デプロイ

GitHub Pages、Branch: main / パス: /(root)。プッシュすれば 1〜3 分で反映。

## 現状と残タスク

- 読む回 32、実践 B1〜B6 は手順書として完成、B7〜B9 は意図的に設計メモ
- コードは実機（Godot 4）未検証。検証して直した場合は、冒頭の断り
  （s0 ではなく index の説明文と、リード文内の .sketch 枠）の更新も検討する
- 作者は Godot 初学者。変更は小さく、1 コミット 1 目的で
