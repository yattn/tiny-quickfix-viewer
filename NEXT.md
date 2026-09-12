# NEXT: quickfix ビューア

`getqflist()` を popup で絞って飛ぶ。状態は持たない。将来の live grep の受け皿。

## やること

- `:Tqv` で `getqflist()` を候補にする
- 候補表示: `path:line:col  text`（相対パス。実測で `text` キーあり）
- `valid` が 0 の項目は除外（`:grep` 由来の pattern 項目は飛べないため）
- 確定: `edit` + `cursor(lnum, col == 0 ? 1 : col)`
- キー操作・見た目は tff と同じ

## スコープ外

- location list（`getloclist()`。必要になったら1語違い）
- quickfix の編集・追加・移動

## テスト

- `setqflist()` で2件入れる → 行数・絞り込み・Enter後のカーソル位置を検証

## 共通の約束（tpf/tff と同じ）

- Vim9のみ・依存なし・KISS・状態最小。`plugin/tqv.vim` + `autoload/tqv.vim` の2ファイル
- 骨格は tff のコピーでよい（popup + filter、先頭行=入力欄、`> `マーカー、border:[]、padding:[0,1,0,1]、20件）
- 2本目以降で共通コア切り出しを判断（時期尚早ならコピーのまま）
- 罠: def引数名とスクリプト変数の衝突不可(E1168)／テストは `--cmd "set rtp+=$PWD"`／`writefile(/dev/stderr)` 禁止
