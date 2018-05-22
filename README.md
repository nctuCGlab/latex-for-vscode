# Writing LaTeX in VSCode
在VSCode上寫LaTeX的安裝記錄，OS以Linux為主、Windows也有裝成功  
注意VSCode與LaTeX Workshop版本

## 參考
[臺灣大學碩博士論文 XeLaTeX 模版](https://github.com/shaform/ntu-thesis/wiki)  
[配置VSCode为LaTeX集成开发环境(IDE) - 初级版](https://zhuanlan.zhihu.com/p/31883018)  
[LaTeX技巧932：如何配置Visual Studio Code作为LaTeX编辑器[新版更新]](http://www.latexstudio.net/archives/12260)  
[Windows 平台下在 Visual Studio Code 上使用 LaTeX](https://eggycat.github.io/2018/02/13/LaTeX-on-vscode/)

## 環境
Linux Mint 18.1 Serena (Ubuntu 16.04)  
Visual Studio Code 1.23.1  
  
Windows 10  
Visual Studio Code 1.23.1  

## TeX Live
主要包含一些LaTeX編譯器
* ### Linux
```shell=
sudo apt-get install texlive texlive-xetex texlive-latex-recommended texlive-latex-extra texlive-bibtex-extra texlive-science texlive-humanities
```
* ### Windows
從[元智的鏡像站](http://ftp.yzu.edu.tw/CTAN/systems/texlive/Images/)下載 **TeX Live 2018** (```texlive2018.iso``` 約3.2GB)  
之後掛載或解壓縮，執行```install-tl-windows.bat```  
安裝過程會問你要裝哪些，可以把前端工具取消不選  
  
裝完後可以跳去 **LaTeX Workshop** 步驟  

## 安裝字型
取自[臺灣大學碩博士論文 XeLaTeX 模版](https://github.com/shaform/ntu-thesis/wiki)  
重點就是要有這5個字型檔
* kaiu.ttf
* timesbd.ttf
* timesbi.ttf
* timesi.ttf
* times.ttf
```shell=
# 4.1. 建立資料夾
sudo mkdir -p /usr/share/fonts/truetype/win/

# 4.2. 從 Windows 複製字體
# 請將 Windows 字型資料夾的 kaiu.ttf 及 times 開頭的檔案放置在 [WINDOWS]/Windows/Fonts/ 資料夾
# 或者是直接掛載 Windows 字型資料夾所在的磁區
sudo cp [WINDOWS]/Windows/Fonts/kaiu.ttf /usr/share/fonts/truetype/win/
sudo cp [WINDOWS]/Windows/Fonts/times*.ttf /usr/share/fonts/truetype/win/

# 4.3. 更新字體
fc-cache

# 4.4. 檢查是否成功，應該要看到標楷體和 Times New Roman
fc-list | grep "times\|kaiu"
```

## fonts-lmodern
理論上按照[臺灣大學碩博士論文 XeLaTeX 模版](https://github.com/shaform/ntu-thesis/wiki)所說  
流程至此應該就可以```make```成功  
但我還是碰到了字型的error，解法如下
```shell=
sudo apt-get install fonts-lmodern
```

## LaTeX Workshop
直接在VSCode擴充功能上安裝 **LaTeX Workshop** 插件  
我安裝的時候版本是5.4.0  
接著 **開啟資料夾->(選擇論文資料夾)**  
再從 **設定->工作區設定**  
加入
```json=
  "latex-workshop.view.pdf.viewer": "tab",
  "latex-workshop.latex.recipes": [
      {
          "name": "xelatex -> bibtex -> xelatex*2",
          "tools": [
              "xelatex",
              "bibtex",
              "xelatex",
              "xelatex"
          ]
      }
  ],
  "latex-workshop.latex.tools": [
      {
          "name": "bibtex",
          "command": "bibtex",
          "args": [
            "%DOCFILE%"
          ]
      },
      {
          "name": "xelatex",
          "command": "xelatex",
          "args": [
              "-synctex=1",
              "-interaction=nonstopmode",
              "-file-line-error",
              "-shell-escape",
              "%DOC%"
          ],
      }
  ],
```
包含了設定在新分頁檢視pdf檔，選擇```XeLaTeX```作為編譯器，及設定編譯流程  

這樣裝完之後  
每次修改tex檔並儲存時，就會自動編譯並刷新已開啟的pdf檔  
點右上角 ```View LaTeX PDF file (Ctrl+Alt+V)```

* ### Windows
一些注意事項  
1. 論文資料夾路徑不得為中文  
2. 如果有碰到類似```Error: Command \counterwithout already defined```  
參考[StackExchange](https://tex.stackexchange.com/questions/425600/latex-error-command-counterwithout-already-defined)  
需在```thesis.tex```最前面加上這兩行
   ```=
   \let\counterwithout\relax
   \let\counterwithin\relax
   ```

## 其他
如果想進行pdf文件處理  
可以參考[臺灣大學碩博士論文 XeLaTeX 模版](https://github.com/shaform/ntu-thesis/wiki)的其他部份
