# 表紙メーカー — Claude への指示

単一HTML（`index.html`）で完結する。外部ライブラリ・ビルド・サーバーを入れない。

## 守ること
- 表紙内の寸法・座標は `cqw` で持つ（mm・px を混ぜると用紙切替で崩れる）。ページ高さは `pageH()`
- 画像データは `state.assets` に置き、要素からは `a`（アセットID）で参照する。要素JSONに dataURL を入れない（undo履歴が肥大する）
- 変更を確定する操作の末尾で `changed()` を呼ぶ（履歴・サムネイル・保存が連動）。ドラッグ中は `geom()` だけ更新する
- ポインターキャプチャ中は `e.target` が `#cv` になる。ダブルクリック等は `document.elementFromPoint` で対象を引く
- `var state` は初期化中の `pageH()` が参照するため意図的に var
- 印刷は `#print` に全ページを組み直して `window.print()`。`@page` サイズは `applyPaper()` が注入する
- 編集後は script を取り出して `node --check`。動作確認は Playwright（http サーバー経由。file:// は不可）で、`page.mouse` による実ドラッグで行う
