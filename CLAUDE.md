# 表紙メーカー — Claude への指示

単一HTML（`index.html`）で完結する。外部依存は Google Fonts の CSS だけ。ビルド・サーバーを入れない。
GitHub: mkt918/hyoushi-maker（public、Pages で配信）。

## 守ること
- 表紙内の寸法・座標は `cqw` で持つ（mm・px を混ぜると用紙切替で崩れる）。ページ高さは `pageH()`、表示用の換算は `mmPer()` / `ptPer()`
- 要素の既定値は `mkText` などのコンストラクタに集約する。項目を足したら `norm()` で旧データにも既定値が入ることを確認する
- 画像データは `state.assets` に置き、要素からは `a`（アセットID）で参照する。要素JSONに dataURL を入れない
- 変更を確定する操作の末尾で `changed()` を呼ぶ（履歴・サムネイル・保存が連動）。ドラッグ中は `touch()` だけで軽く更新する
- ポインターキャプチャ中は `e.target` が `#cv` になる。ダブルクリック等は `document.elementFromPoint` で対象を引く
- `var state` は初期化中の `pageH()` が参照するため意図的に var
- 印刷は基本グレースケール。新しいテンプレートはグレーだけで組み、白黒でも文字と背景の明暗差が十分あることを確かめる
- テンプレートの文字要素には `role`（title / notes / sub / sub2）を付ける。切り替え時の引き継ぎはこれで対応づける
- 印刷は `#print` に全ページを組み直して `window.print()`。`@page` サイズは `applyPaper()` が注入する

## 検証
- script を取り出して `node --check`
- 動作確認は Playwright（`python -m http.server` 経由。file:// は不可）で、`page.mouse` による実ドラッグで行う。印刷ボタンは押さない（ダイアログで止まる）
- UI のスクリーンショットはユーザーに送らない（ユーザーが自分のブラウザで見る）
