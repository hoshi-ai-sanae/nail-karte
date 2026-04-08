# 💅 ネイルサロン顧客管理カルテ

GitHub Pages で動く、ネイリスト向け顧客管理アプリです。
サーバー不要・APIキー不要で、すぐに使い始められます。

---

## 📁 ファイル構成

```
nail-karte/
├── customer-form.html   ← お客様用アンケート（公開URLで共有）
├── login.html           ← ネイリストログイン
├── dashboard.html       ← ダッシュボード（フォロー一覧）
├── clients.html         ← 顧客一覧・検索
├── client-detail.html   ← 顧客詳細・来店記録・アドバイス
├── settings.html        ← 設定（サロン名・PW・GAS連携）
└── README.md
```

---

## 🚀 GitHub Pages へのアップ手順

1. GitHubで新しいリポジトリを作成（例: `nail-karte`）
2. 全ファイルをアップロード（「Add file → Upload files」）
3. Settings → Pages → Branch: main → Save
4. 公開URL が発行されます：
   `https://あなたのID.github.io/nail-karte/`

---

## ⚙️ 初期設定

1. `settings.html` にアクセス
2. サロン名・ネイリスト名を入力して保存
3. パスワードを変更（初期値: `nail1234`）
4. お客様用URL を共有：
   `https://あなたのID.github.io/nail-karte/customer-form.html`

---

## 📊 Googleスプレッドシート連携（任意）

Googleスプレッドシートを開き、拡張機能 → Apps Script に以下を貼り付けてデプロイ：

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet();
  const data = JSON.parse(e.postData.contents);

  if (data.type === 'new_client') {
    const d = data.data;
    const s = sheet.getSheetByName('顧客') || sheet.insertSheet('顧客');
    if (s.getLastRow() === 0) {
      s.appendRow(['登録日','氏名','電話','連絡先','誕生日','メニュー','爪のお悩み','生活スタイル','デザイン系統','備考']);
    }
    s.appendRow([d.submittedAt, d.name, d.phone, d.contact, d.birthday, d.menu, d.nailConcerns, d.lifestyle, d.designStyle, d.notes]);
  }

  if (data.type === 'visit_record') {
    const r = data.record;
    const s = sheet.getSheetByName('来店記録') || sheet.insertSheet('来店記録');
    if (s.getLastRow() === 0) {
      s.appendRow(['顧客ID','施術日','メニュー','使用カラー','爪メモ','次回提案','インスタURL']);
    }
    s.appendRow([data.clientId, r.date, r.menu, r.color, r.nailMemo, r.nextMemo, r.instaUrl]);
  }

  return ContentService.createTextOutput(JSON.stringify({status:'ok'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

デプロイ後に発行されたURLを `settings.html` に貼り付けてください。

---

## 🔒 セキュリティについて

- パスワードはシンプルなローカルストレージ認証です
- 顧客データは端末のlocalStorageに保存されます
- 外部サーバーへの送信はGASのURLのみです（任意設定）
- 本格運用の場合はサーバーサイド認証の導入をご検討ください

---

## 📱 動作環境

- Chrome / Safari / Firefox（最新版）
- スマートフォン対応（レスポンシブ）
- GitHub Pages で動作（サーバー・APIキー不要）
