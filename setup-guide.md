# 清流農園 LINE リッチメニュー 設定手順

## ファイル一覧

| ファイル | 内容 |
|---|---|
| `richmenu.html` | 仕様書（ブラウザで確認用） |
| `richmenu-image.svg` | リッチメニュー画像（2500×1686px） |
| `richmenu-api.json` | LINE Messaging API 登録用JSON |

---

## STEP 1: 画像の準備

`richmenu-image.svg` をPNGに変換します。

**変換方法（いずれか）:**
- [Squoosh](https://squoosh.app/) でSVGを開いてPNG書き出し
- Figmaにインポート → PNG書き出し（2x）
- `inkscape richmenu-image.svg --export-png=richmenu-image.png -w 2500`

**画像仕様:**
- サイズ: 2500 × 1686 px
- 形式: PNG または JPEG
- 容量: 1MB以下

---

## STEP 2: LINE Messaging API でリッチメニュー登録

### 2-1. リッチメニューを作成

```bash
curl -X POST https://api.line.me/v2/bot/richmenu \
  -H "Authorization: Bearer {チャネルアクセストークン}" \
  -H "Content-Type: application/json" \
  -d @richmenu-api.json
```

レスポンスで `richMenuId` が返ってきます（例: `richmenu-xxxxxx`）。

### 2-2. 画像をアップロード

```bash
curl -X POST https://api-data.line.me/v2/bot/richmenu/{richMenuId}/content \
  -H "Authorization: Bearer {チャネルアクセストークン}" \
  -H "Content-Type: image/png" \
  --data-binary @richmenu-image.png
```

### 2-3. デフォルトリッチメニューに設定

```bash
curl -X POST https://api.line.me/v2/bot/richmenu/default/{richMenuId} \
  -H "Authorization: Bearer {チャネルアクセストークン}"
```

---

## チャネルアクセストークンの取得

1. [LINE Developers Console](https://developers.line.biz/) にログイン
2. 清流農園のチャネル → 「Messaging API設定」
3. 「チャネルアクセストークン」を発行・コピー

---

## 動作確認

登録後、LINEアプリでトークルームを開くと下部にリッチメニューが表示されます。

| ボタン | 動作 |
|---|---|
| 🛒 TikTok Shop | TikTok Shopの商品一覧を開く |
| 📦 BASEショップ | BASEショップを開く |
| 🏪 直売所情報 | 営業時間・住所・マップのテキストを送信 |
| 📱 SNS一覧 | TikTokアカウントURLを送信 |
