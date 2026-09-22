# Tightcut 法務文書

| 項目 | 内容 |
| --- | --- |
| 作成日 | 2026-09-23 |
| 更新日 | 2026-09-23 |
| バージョン | 2026.09.23 |
| 作成者 | 大森 |
| 更新者 | 大森 |

iOS アプリ「Tightcut」（動画の無音カット・買い切り）のプライバシーポリシー・利用規約・サポートを
公開するための GitHub Pages 用リポジトリです。Jira: **KAN-833**（エピック KAN-814）。

メインのコードリポジトリ: https://github.com/naruberio/tightcut-app（Private）

## 公開 URL

| 用途 | URL |
| --- | --- |
| プライバシーポリシー | https://naruberio.github.io/tightcut-legal/privacy-policy |
| 利用規約 | https://naruberio.github.io/tightcut-legal/terms |
| サポート | https://naruberio.github.io/tightcut-legal/support |
| 日本語版 | `/privacy-policy-ja` / `/terms-ja` / `/support-ja` |

App Store Connect には Privacy Policy URL に `/privacy-policy`、Support URL に `/support` を登録する。

**拡張子を書かない形で登録する。** アプリ本体（`SettingsView.swift`）が
`/privacy-policy` と `/terms` にリンクしており、GitHub Pages は `foo.html` を `/foo` でも返す
（2026-09-23 に mixroll-legal / raypin-legal / silencut-legal の 3 本で 200 を実測）。

## 正本は HTML

Markdown + Jekyll ではなく **HTML を正本**にする（mixroll-legal と同じ運用・KAN-833）。
`.nojekyll` を置いて GitHub Pages のビルドを通さない。**アプリ側に Markdown の写しを置かない**
（二重管理になり、審査に出した版とずれる）。

スタイルは `style.css` 1 本に寄せている（ページごとに `<style>` を写すと 7 か所の重複になる）。

## 言語

- 英語（正本）: `privacy-policy.html` / `terms.html` / `support.html`
- 日本語: `privacy-policy-ja.html` / `terms-ja.html` / `support-ja.html`

アプリ本体の掲載文は 20 言語だが、法務文書は既存 15 本と同じく en / ja の 2 言語で運用する。

## 改訂方針

App Store 提出版と一致させる。とくに次の 3 点は `tightcut-app` 側の実装と食い違わせない。

| 記述 | 実装側の根拠 |
| --- | --- |
| 通信ゼロ・第三者 SDK ゼロ | Swift ソースに `URLSession` / `http` の参照が 0 件 |
| 写真ライブラリは**書き込みのみ** | `PhotoLibrarySaver.swift` の `.addOnly` / `Info.plist` に `NSPhotoLibraryAddUsageDescription` のみ |
| Required Reason API は 2 つ | `PrivacyInfo.xcprivacy` の `CA92.1` / `0A2A.1` |

App Privacy（ASC の「Appのプライバシー」）の公開は **API では行えない**（ASC の UI 操作）。
ここに書いた内容と ASC 側の申告を食い違わせないこと。

アプリ側の設定画面からこの URL へリンクしているので、リンク先を変える場合は
`tightcut-app` の `Tightcut/Sources/Features/Settings/SettingsView.swift` も併せて直す。

## サポート連絡先

konpei.work@gmail.com
