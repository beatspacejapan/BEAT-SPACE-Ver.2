# BEAT SPACE — リニューアル プロトタイプ

Producer / Artist / DJ に特化した「音楽 Discovery × Marketplace × Media」のプロトタイプです。
`index.html` 1ファイルで完結しており、ビルド不要・サーバー不要で動きます。

**デモURL**: https://USERNAME.github.io/beatspace/ ← 公開後に書き換えてください

---

## 公開手順（GitHub Pages）

### ブラウザだけで進める場合

1. GitHub で新しいリポジトリを作成（例: `beatspace`）。Public を選択します。
2. リポジトリの **Add file → Upload files** を開き、このフォルダの中身をすべてドラッグ&ドロップ
   （`index.html` / `README.md` / `.nojekyll` / `assets` フォルダ）
3. **Commit changes** を押す
4. **Settings → Pages** を開く
5. Source で **Deploy from a branch** を選び、Branch を `main` / フォルダを `/ (root)` にして Save
6. 1〜2分待つと `https://ユーザー名.github.io/リポジトリ名/` で公開されます

### コマンドで進める場合

```bash
git init
git add .
git commit -m "BEAT SPACE prototype"
git branch -M main
git remote add origin https://github.com/USERNAME/beatspace.git
git push -u origin main
```

その後、Settings → Pages から上記5の設定を行ってください。

### 公開後にやること

`index.html` の冒頭にある以下2行の `USERNAME` を、自分のGitHubユーザー名とリポジトリ名に書き換えます。
SNSでシェアしたときのサムネイル（OGP画像）の表示に使われます。

```html
<meta property="og:url" content="https://USERNAME.github.io/beatspace/">
<meta property="og:image" content="https://USERNAME.github.io/beatspace/assets/og.jpg">
```

独自ドメイン（例: `demo.beat-space.com`）を使う場合は、`CNAME` という名前のファイルを作り、
中身にドメイン名だけを書いて置き、DNS側で CNAME レコードを `USERNAME.github.io` に向けます。

---

## ファイル構成

```
.
├── index.html          プロトタイプ本体（画像・データすべて内包）
├── assets/
│   ├── og.jpg          SNSシェア用サムネイル（1200×630）
│   └── favicon.png     ファビコン
├── .nojekyll           GitHub Pages の Jekyll 処理を無効化
└── README.md
```

---

## 実装されている機能

**Discover / Marketplace**
- 固定グローバルプレイヤー（ページ移動しても再生継続・連続再生・シャッフル・波形シーク・再生キュー）
- 検索とフィルター（Genre / Mood / BPM / Key / キーワード / 並び替え）
- ビート詳細、Non-Exclusive / Exclusive のライセンス選択、モック決済（手数料10%と受取額の内訳表示）
- 購入完了後の著作権譲渡証明書の発行（印刷・PDF保存対応）
- 出品フォーム（公開するとその場でカタログに追加されます）

**会員機能**
- ログイン / マイページ（購入履歴・お気に入り・フォロー・プレイリスト・通知設定）
- 購入額に応じたランク表示
- BEATSPACE PRO の加入 / 解約

**メディア / 収益**
- STATION（ラジオアーカイブ・DJ MIX・PRODUCER MIX）
- イベント（フライヤー・チケット導線・コンテスト応募フォーム・ギャラリー）
- BEATSPOT（縦型ショート）
- 音声広告（FREEプランで数曲に1回、プレイヤーが広告表示に切り替わります）
- 広告メニュー / セルフサーブ広告の申し込み

---

## 注意事項

- **プロトタイプです。** 決済は Stripe のモックで、実際の課金は発生しません。音源は再生を模した動作で、音は鳴りません。
- プロデューサーのプロフィール文・使用機材・実績・フォロワー数は仮のテキストです。
- 掲載している写真・ジャケット・フライヤー・ロゴは BEAT SPACE の所有素材です。第三者による二次利用はご遠慮ください。
- 検索エンジンに載せたくないため `<meta name="robots" content="noindex">` を入れています。一般公開する際は削除してください。
- ページ内リンクは URL を変更しない内部ルーターで動いています。ブラウザの戻るボタンではなく、画面上のメニューから移動してください。

---

## 本番実装への移行メモ

現在は単一HTMLですが、本番は以下の構成を想定しています。

| 領域 | 想定 |
|---|---|
| フロント | Next.js + Vercel |
| DB / 認証 / 音源保管 | Supabase |
| 決済・分配 | Stripe + Stripe Connect（手数料10%の自動分配） |
| 動画 | 外部配信（HTMLへの埋め込みはファイルが重くなるため） |

移行時は、このプロトタイプの `BEATS` / `PRODUCERS` / `MIXES` / `EVENTS` / `SHORTS` の各配列が、
そのままデータベースのテーブル設計の下敷きになります。
