# 台灣廟宇地圖

運用三種方式優化讀取速度，讓原本需要讀取7秒的網頁減少到1.5秒
1. 不要讓框架渲染過長的陣列（減少4秒）
本來是用Vue的v-for render所有方格，但效率極差。由於這些方格不需要reactivity，之後改用d3在mounted render方格。
2. 盡量簡化地圖（減少2.5秒）
由於這頁總共要render 6 * 2 = 12個台灣地圖，因此勁量簡化地圖圖資，可以大量減少讀取時間。原本用mapshaper簡化到1%（？），後來又再簡化一次到10%，大幅提升讀取速度。
3. 盡量減少get Map的次數（可能有減少讀取時間，但極小）
本來get colorScale的位子在fill的地方，但這樣就要在處理每個方格的時候都get一次，沒什麼必要。改在forEach function的開頭定義新變數color，就只要get 6次，雖然這個更動的效能提升不太明顯，可能是因為Map的效能很高。

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
