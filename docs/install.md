# Install

雖然可以使用 CDN 的方式來引用 Tailwindcss ，但 Tailwindcss 強大的地方就是有使用到的 class 才會加入到 css 之中，這樣大大減少了 css 檔案的大小，不在需要引用一整包大包的 css 檔案，但這個功能需要一些步驟來安裝。



### 安裝 Tailwindcss

雖然在 html 檔案裡面還是可以透過官方的 CDN 方式來引入 css 檔案，但因為透過 CDN 方式所載入的 css 檔案極大，而且會包含沒有用到的功能。而在 tailwind v3 的版本中，會根據 html 內有用到的功能來產生對應的 css 檔案，有需要才打包出來，非常符合傳輸效益。

### 安裝步驟

安裝的所有步驟都會使用終端機來執行命令，開啟終端機之後就可以開始以下的步驟。



1 新建資料夾

```shell
$ mkdir tailwindcss-demo
```



2 專案初始化

```shell
$ npm init -y
```

這邊使用 npm 來安裝， -y 表示跳過所有提示步驟，如果不輸入 -y 則會需要一個一個的問題確認，這些問題是關於這個專案的一些資訊，通常都會直接略過而使用初始化的內容。



3 安裝套件

```shell
$ npm i -D tailwindcss postcss autoprefixer
```

這邊安裝 tailwindcss 的版本會是 v3.1.8，而 postcss 與 autoprefixer 則是 tailwind 編譯時會使用到的兩個產生 css 的工具，這邊就不多做介紹。



4 產生配置檔案

```shell
$ npx tailwindcss init -p
```

輸入完之後會產生兩個檔案：tailwind.config.js 與 postcss.config.js。

- tailwind.config.js：會放相關的 html 檔案與 js 檔案路徑，或者一些客製化的設定。
- postcss.config.js：會放 postcss 插件的設定。



5 新增 tailwind.css 檔案

接下來稍微整理一下檔案，先新增一個資料夾 src，然後把 tailwind.css 放在資料夾 src 底下，並且輸入：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

編譯 tailwind 時，會參考這個檔案的編譯來源，來產生對應的 css 功能。



6 新增 index.html 檔案

新增一個資料夾 dist，然後把 index.html 這個檔案放在資料夾 dist 底下，並且輸入：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link href="style.css" rel="stylesheet">
</head>

<body>
    <h1 class="text-3xl font-bold underline">
        Hello world!
    </h1>
</body>

</html>
```

這邊會發現還沒有產生 style.css，所以開啟這個檔案是沒有任何效果的。而在這個時候 tailwind.config.js 就會發揮功能，tailwind v3 會檢查 tailwind.config.js 內的 html 與 js 相關檔案的路徑，而去檢查這些檔案內所引用到的 tailwind 的功能，經由編譯之後統一放到所輸出的 css 檔案內。簡單講就是有用到的 class 功能，才會產生對應的 class。



7 修改 tailwind.config.js

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}", "./dist/**/*.html"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

這邊的 content 就是設定要編譯的檔案範圍，src 資料夾內可以放入自己加入的 js 檔案，這邊先不放入任何 js 檔案。因為我將  index.html 放在 dist 這個資料夾內，所以需要多放入這個資料夾內的所有 html 檔案。



8 編譯

```shell
$ npx tailwindcss -i ./src/tailwind.css -o ./dist/style.css
```

編譯成功後會產生 dist/style.css ，直接用瀏覽器打開 index.html 就可以看到結果。



### 指令

為了編譯方便，可以將編譯指令放入到 package.json：

```json
"scripts": {
    "build": "tailwindcss -i ./src/tailwind.css -o ./dist/style.css"
},
```

然後執行以下指令可以重新編譯

```shell
$ npm run build
```

上面編譯的方式，是每次修改 HTML 時，都要重新執行編譯指令，其實相當不方便。如果希望 HTML 檔案存擋之後可以自動編譯，可以新增指令到 package.json：

```json
"scripts": {
    "watch": "tailwindcss -i ./src/tailwind.css -o ./dist/style.css --watch"
},
```

然後執行：

```shell
$ npm run watch
```
