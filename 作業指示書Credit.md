# 作業指示書：クレジット情報の追加

## 概要
`the-future.sh`プロジェクトに作者情報（クレジット）を追加する。`help`コマンドと新規作成する`about`コマンドの両方に表示する。

## 追加する情報
- X (Twitter) アカウント: @poknam
- GitHub アカウント: kanekop
- プロジェクトURL: https://github.com/kanekop/the-future-sh

## 作業内容

### 1. helpコマンドの更新

`index.html`内の`help`コマンド処理部分（`} else if (cmd === 'help') {`）を以下のように更新：

```javascript
} else if (cmd === 'help') {
    addLine(t('availableCommands'));
    addLine(t('cmdBootFuture'));
    addLine(t('cmdInteractive'));
    addLine(t('cmdHelp'));
    addLine(t('cmdLs'));
    addLine(t('cmdCat'));
    addLine(t('cmdClear'));
    addLine(t('cmdPwd'));
    addLine(t('cmdWhoami'));
    addLine(t('cmdHelpCmd'));
    addLine('  about                                                     - About this project');
    addLine('  lang [ja|en]                                              - Switch language');
    addLine('');
    addLine('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━');
    addLine('the-future.sh v1.0');
    addLine('Created by Kaneko (<span class="log-info">@poknam</span>)');
    addLine('https://github.com/kanekop/the-future-sh');
    createInputLine();
    return;
}
```

### 2. aboutコマンドの新規追加

`help`コマンドの処理ブロックの後に、新しく`about`コマンドを追加：

```javascript
} else if (cmd === 'about') {
    addLine('the-future.sh - A tiny CLI simulator to boot your future');
    addLine('');
    addLine('Version: 1.0');
    addLine('');
    addLine('Created with ❤️ by Kaneko');
    addLine('');
    addLine('🐦 X (Twitter): <span class="log-info">https://x.com/poknam</span>');
    addLine('🐙 GitHub: <span class="log-info">https://github.com/kanekop</span>');
    addLine('📦 Project: <span class="log-info">https://github.com/kanekop/the-future-sh</span>');
    addLine('');
    addLine('<span class="log-tip">Remember: A vision without a commit is just a dream.</span>');
    createInputLine();
    return;
}
```

### 3. Tab補完の更新

Tab補完で利用可能なコマンドリストに`about`を追加。`handleTabCompletion`関数内：

```javascript
const commands = ['cat', 'ls', 'clear', 'pwd', 'whoami', 'help', 'about', 'lang', 'future', './future', 'future.sh', './future.sh'];
```

### 4. スタイルの確認

既存のスタイルクラスを使用：
- `log-info`（青色）: リンクやハイライト部分
- `log-tip`（グレー）: 補足情報

## テストチェックリスト

- [ ] `help`コマンドでクレジット情報が表示される
- [ ] `about`コマンドが正常に動作する
- [ ] `about`のTab補完が機能する
- [ ] 日本語環境でも正常に表示される
- [ ] リンクの色が適切に表示される（`log-info`クラス）
- [ ] レイアウトが崩れていない

## 注意事項

- URLはクリッカブルではありません（ターミナルの仕様として）
- 文字コードは既存のコードベースに合わせてください
- インデントやコーディングスタイルは既存のコードに準拠してください

