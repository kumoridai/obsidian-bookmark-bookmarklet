# Obsidian Bookmarklet

このリポジトリは、ウェブページの情報（タイトル、URL、閲覧日）をメモアプリ **Obsidian** に自動で記録するブックマークレットです。ブックマークレットを実行すると、指定のフォルダに現在のページの情報を保存し、クリップボードにWikiリンクをコピーします。

## 機能

- 現在のウェブページのタイトルとURLを自動取得
- ISO形式またはカスタム形式の日付を含めたメタデータを生成
- Obsidianで使えるWikiリンクを生成し、クリップボードにコピー
- Obsidianの指定フォルダにMarkdownファイルを作成

## インストール

1. このブックマークレットをコピーします。

javascript:(function(){const now=new Date();const pad=(num)=>String(num).padStart(2,'0');const formattedDate=`${now.getFullYear()}-${pad(now.getMonth()+1)}-${pad(now.getDate())}T${pad(now.getHours())}:${pad(now.getMinutes())}:${pad(now.getSeconds())}`;const wikiDate=`[[${now.getFullYear()}_${pad(now.getMonth()+1)}_${pad(now.getDate())}]]`;const title=document.title;const url=document.URL;const bookmarkFolder="bookmarks/";const invalidChars=/[<>:"\/\\|?*#]/g;const safeFileName=title.replace(invalidChars,'_');const bookmarkFileName=%60${safeFileName}%60;const bookmarkFileContent='---\n'+%60url: ${url}\n%60+%60date: ${formattedDate}\n%60+'---\n'+wikiDate+'閲覧\n';const obsidianUrl="obsidian://new?"+"file="+encodeURIComponent(bookmarkFolder+bookmarkFileName)+"&content="+encodeURIComponent(bookmarkFileContent);const wikiLink=%60[[${safeFileName}]]%60;navigator.clipboard.writeText(wikiLink);document.location.href=obsidianUrl;})();

2. Webブラウザでブックマークを作成し、そのURL部分に上記のコードを貼り付けます。

## 使い方

1. 任意のウェブページを開きます。
2. 作成したブックマークをクリックします。
3. 自動でObsidianが開き、指定のフォルダにwebページ名をタイトルに持つ新規のMarkdownファイルが作成されます。
4. 生成されるページはプロパティとしてURLと閲覧日時を、ページコンテンツとして閲覧日をwikiリンク形式で持っています。その日のデイリーノートからバックリンクで閲覧できます。
5. クリップボードにwikiリンク形式にしたページタイトルがコピーされるので、デイリーノートなど必要な場所に貼り付けることもできます。

## カスタマイズ

### 日付のフォーマット

日付のフォーマットは、本文にwikiリンクとして書き込む日時を`wikiDate`、プロパティに記載する閲覧日時を`formattedDate`で定義されています。
デフォルトではアンダーバー区切りの日付形式(`YYYY_MM_DD`)とISO形式（`YYYY-MM-DDTHH:MM:SS`）が使われていますが、以下のように変更することができます。
作者のデイリーノートの区切りはアンダーバーですが、Obsidianのデフォルト設定はハイフン区切りのため、適宜変更を加えてください。

- `formattedDate`: 保存されるファイルに書かれる日付のフォーマット
- `wikiDate`: Obsidian内で使うWikiリンクのフォーマット

例: `YYYY-MM-DD`及び`YYYY/MM/DD HH:MM:SS`に変更する場合

const wikiDate=`[[${now.getFullYear()}-${pad(now.getMonth()+1)}-${pad(now.getDate())}]]`;
const formattedDate=`${now.getFullYear()}/${pad(now.getMonth()+1)}/${pad(now.getDate())} ${pad(now.getHours())}:${pad(now.getMinutes())}:${pad(now.getSeconds())}`;

### ブックマークフォルダのパス

保存先フォルダは、`bookmarkFolder`変数で指定されています。デフォルトでは`"bookmarks/"`となっていますが、ユーザーのObsidian環境に合わせて以下のように変更してください。

const bookmarkFolder="MyNotes/Bookmarks/";

## 注意

- Obsidianがインストールされており、`obsidian://new?`スキームが正しく設定されている必要があります。
- ブックマークレットが正しく動作するために、Webブラウザが`obsidian://`リンクを許可している必要があります。

## ライセンス

このプロジェクトはMITライセンスの下で提供されています。
