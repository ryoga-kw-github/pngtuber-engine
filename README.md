# 📺 PNGTuber Engine

**Webアプリ、ゲーム、配信画面に「ポン付け」できる、PNGTuber埋め込みエンジン。**

`pngtuber-engine` は、1つのHTMLファイル (`essential-pngtuber.html`) で動作する、軽量かつ高機能なアバター表示システムです。
外部ライブラリへの依存は一切ありません（Vanilla JS 100%）。

## ✨ Features (特徴)

- **Zero Dependencies**: 依存ライブラリなし。非常に軽量。
- **Iframe Sandbox**: `iframe` で埋め込むため、親ページのCSSやJSと競合しません。
- **Full API Control**: マイク入力、音声ファイル再生、表情切り替え、動きの調整など、すべてJavaScriptから制御可能。
- **Silent Lip-sync**: 音声を出さずに（ミュート状態で）波形解析だけ行い、口パクさせることが可能。AIチャットのフロントエンドに最適。
- **JSON Configuration**: 設定ファイルやURLパラメータでの一括設定に対応。

## 🚀 Quick Start

### 1. Install
`essential-pngtuber.html` をあなたのプロジェクトの適当なフォルダ（例: `/assets/`）にコピーしてください。

### 2. Embed (埋め込み)
使いたい場所に `iframe` タグを書くだけです。

```html
<!-- URLパラメータで設定する場合 -->
<iframe 
  src="assets/essential-pngtuber.html?closedOpen=idle.png&openOpen=talk.png&fit=contain"
  style="width: 300px; height: 300px; border: none;">
</iframe>
```

### 3. Control (制御)
JavaScriptで自由に操作できます。

```javascript
const iframe = document.querySelector('iframe');
iframe.onload = () => {
  const engine = iframe.contentWindow.pngtuber;

  // マイク入力を開始
  engine.startMicrophone();

  // 設定や画像を動的に変更
  engine.setConfig({
    boundStrength: 20, // 激しく揺れる
    images: {
      closedOpen: 'img/angry_idle.png',
      openOpen:   'img/angry_talk.png'
    }
  });
};
```

## 🛠 PNGTuber Studio (Generator)

同梱の `pngtuber-studio.html` をブラウザで開くと、GUIでパラメータを調整し、設定用JSONファイルを生成できます。
作成したJSONは以下のように読み込めます。

```html
<iframe src="essential-pngtuber.html?config=my-avatar.json"></iframe>
```

## 📚 API Reference

`iframe.contentWindow.pngtuber` オブジェクトが公開しているメソッドです。

### `setConfig(config: Object)`
設定や画像を更新します。必要なプロパティだけを渡せばマージされます。

```javascript
engine.setConfig({
  threshold: 0.05,      // 口パクの感度 (デフォルト: 0.02)
  boundStrength: 10,    // 縦揺れの強さ px (デフォルト: 8)
  boundSpeed: 0.8,      // 揺れの速さ (デフォルト: 0.8)
  fit: 'contain',       // 画像のフィットモード (contain, cover)
  images: { ... }       // 画像パスを一括変更
});
```

### `startMicrophone()`
ブラウザのマイク入力を取得し、リップシンクを開始します。

### `loadAndPlayAudioFromUrl(url, loop=false, muted=false)`
指定したURLの音声ファイルを再生し、リップシンクさせます。
*   **`muted: true`** にすると、**音は鳴らさずに口パクだけ**します。
    *   親ページ側で音声を再生し、アバターには口パクだけさせたい場合（AIアシスタント等）に便利です。

### `setSpeaking(boolean)` / `setBlink(boolean)`
手動で「口を開く/閉じる」「目を閉じる/開く」を制御します。
音声を使わないレトロゲーム風の演出（ポポポ音など）に使えます。

## 🧩 Use Cases

### for AI Chat Bots
ChatGPTなどのAIからの応答音声を再生しつつ、アバターを動かせます。

### for Visual Novel Games
Webベースのノベルゲームの立ち絵として。
シーンに合わせて `setConfig` で表情を一瞬で切り替えたり、感情に合わせて揺れ幅を変えたりできます。

### for OBS Overlay
背景色を `transparent` にしているため、OBSの「ブラウザソース」として追加するだけで、VTuberのような配信画面が作れます。

## 📜 License
MIT License
```
