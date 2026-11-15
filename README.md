# Tightcut 法務文書

| 項目 | 内容 |
| --- | --- |
| 作成日 | 2026-09-23 |
| 更新日 | 2026-11-15 |
| バージョン | 2026.11.15 |
| 作成者 | 大森 |
| 更新者 | Claude（@大森 の指示・KAN-1242） |

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

## 用途別のページ（KAN-1242・案）

検索や AI の回答から拾われる入口として、用途別のページを 2 用途 × en / ja の 4 枚置く。見た目は `howto.css`（法務ページの `style.css` とは分ける）。

| 用途 | en | ja | App Store へのリンクの `ct` |
| --- | --- | --- | --- |
| 動画の無音・間を自動でカットする | `howto-cut-silence-video.html` | `howto-cut-silence-video-ja.html` | `tcweb-silence-en` / `tcweb-silence-ja` |
| ポッドキャスト・録音の無音を詰める | `howto-trim-pauses-audio.html` | `howto-trim-pauses-audio-ja.html` | `tcweb-audio-en` / `tcweb-audio-ja` |

- 書いてよいのは**公開中の版に入っている機能だけ**（2026-11-15 時点は 1.0.1 = タグ `tf-004` の `ebabde7`。人の声の判定（KAN-1235）と話し言葉の候補（KAN-1236）は 1.0.1 に入っていないので書かない）。
  ボタン名は `Shared/Resources/Localizable.xcstrings` の en / ja と一字一句そろえる。アプリの文言が変わったら同じ作業で直す
- **無料の代替手段へ読者を送る記述と、Tightcut 自身の制約（端末・OS・同時に使えない組み合わせ・上限など）を書かない**（soundpull-app D-58・D-59 を全アプリに当てる）。制約はサポートのページと利用規約に置く
- **価格・課金の形も書かない**（tightcut-app `docs/store-metadata.md` §3。価格を変えてもページを直さずに済むように）
- 各ページの JSON-LD（`HowTo` と `FAQPage`）は、本文の手順と問答から文を取る。本文を変えたら JSON-LD と `dateModified` も直す
- App Store へのリンクは `https://apps.apple.com/app/apple-store/id6816324988?ct=<上の値>&mt=8`。`pt` が無いので Analytics の Campaign には計上されない（FrameSmoother・Readglide と同じ）
- `sitemap.xml` は手で持つ（Jekyll を通さないため）。ページを足したら同じ作業で直す。Search Console・Bing への登録は @大森 の画面の操作

## 言語

- 英語（正本）: `privacy-policy.html` / `terms.html` / `support.html`
- 日本語: `privacy-policy-ja.html` / `terms-ja.html` / `support-ja.html`

アプリ本体の掲載文は 20 言語だが、法務文書は既存 15 本と同じく en / ja の 2 言語で運用する。

## 改訂方針

App Store 提出版と一致させる。とくに次の記述は `tightcut-app` 側の実装と食い違わせない。

| 記述 | 実装側の根拠 |
| --- | --- |
| 通信ゼロ・第三者 SDK ゼロ | Swift ソースに `URLSession` が 0 件。`https://` は設定画面（`SettingsView.swift`）の `Link` 2 件のみで、ブラウザで開く。外部パッケージは `project.yml` の `packages: {}`（0 件） |
| 写真ライブラリは**書き込みのみ** | `PhotoLibrarySaver.swift` の `.addOnly` / `Info.plist` に `NSPhotoLibraryAddUsageDescription` のみ |
| Required Reason API は 2 つ | `PrivacyInfo.xcprivacy` の `CA92.1` / `0A2A.1` |
| 写真の動画は一時フォルダへ複製、ファイルは置き場所のまま読む | `ImportView.swift`（`PickedMedia` が `TemporaryWorkspace.inputsRoot` へ複製・`fileImporter` はセキュリティスコープで読む） |
| 複製は別の素材を開く／「最初から」で消え、遅くとも次の起動で消える | `AppModel.swift` の `open` / `startOver`（`TemporaryWorkspace.purgeInputs`）・`TightcutApp.swift` の `TemporaryWorkspace.purgeStaleSessions()` |
| 仕上がりは Application Support に最新 1 本だけ・バックアップ対象外・起動時に片づける | `ExportStore.swift`（`isExcludedFromBackup`）・`TightcutApp.swift` の `ExportStore.purge()` |
| UserDefaults に覚える値の一覧 | `AppModel.swift` の `Keys` と、切り抜きの精度の `SegmentationQuality.storageKey`（`BackgroundVideoPipeline.swift`。読み書きは `AppModel.swift`）。**`Keys` だけを見ると 1 つ漏れる** |

App Privacy（ASC の「Appのプライバシー」）の公開は **API では行えない**（ASC の UI 操作）。
ここに書いた内容と ASC 側の申告を食い違わせないこと。

アプリ側の設定画面からこの URL へリンクしているので、リンク先を変える場合は
`tightcut-app` の `Tightcut/Sources/Features/Settings/SettingsView.swift` も併せて直す。

## 他アプリの法務ページを写さない（KAN-891）

2026-10-06 に 6 ページすべてを書き直した。Guideline 4.3(a) はアカウント内で似ているだけでも対象になるため、
**見出しの順・決まり文句・段落の組み方を、ほかの `*-legal` リポジトリから写さない。**

改訂するときも、実質の項目（無保証・責任の制限・消費者保護・準拠法と管轄など）は残したまま、
文面は Tightcut の処理の流れに沿って書く。改訂後は、ほかの `*-legal` の同じ種類のページと
重なりを測る（アプリ名を同じ記号に置き換え、英語は 8 語以上・日本語は 20 字以上の一致を数える）。
目安は、ほかのアプリどうしの対の中央値以下。

## サポート連絡先

konpei.work@gmail.com
