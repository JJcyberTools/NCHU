# 1111_DIC4 — IEEE Conference Draft with MCP and OpenSpec

老師要求：1. Overleaf draft for IEEE conference；2. 修改後版本；必須有 Article Summary Table。

已在本資料夾提供初稿和修改後的 LaTeX 投稿草稿。**源檔由 Proposal Document.pdf、Proposal PPT.pdf、DESIGN.md 重建，並非未取得的原始 Overleaf 原始碼。** 全文為 Proposal 階段，沒有虛構實驗結果。

## Overleaf 使用方式

1. 開啟 [IEEE Conference Template](https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn)，使用模板建立自己的專案。
2. 上傳 `02_revised_conference.tex`，設定為主檔（或改名 `main.tex`）。
3. Compiler 選 `pdfLaTeX`，按 Recompile。
4. 將第一版 `01_initial_draft.tex` 與修訂版保留為不同檔案或 Version History。
5. Share 中取得實際 Overleaf 網址，並附 GitHub 及 PDF。尚未取得你的 Overleaf 編輯權限，不能擅自宣稱已上傳到該專案。

## 本機安裝

```bash
sudo apt update
sudo apt install -y texlive-latex-base texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended texlive-pictures latexmk
pdflatex 02_revised_conference.tex
pdflatex 02_revised_conference.tex
```

## OpenSpec 安裝

安裝 Node.js 20.19.0+ 後：

```bash
node --version
npm install -g @fission-ai/openspec@latest
openspec --version
openspec init
```

官方參考：https://openspec.dev/docs/installation

MCP 作用是讓授權工具協助讀寫 GitHub 等資源，不代表自動具有 Overleaf 編輯權限。OpenSpec 管理規格與修改任務，不是排版工具。

## 交件清單

- [x] IEEEtran conference 初稿與修訂稿
- [x] Article Summary Table（AI 六篇、Domain 六篇）
- [x] 九模組、T/F/U、時間推理與 Pseudocode
- [x] OpenSpec 規格草案及編譯指令
- [ ] 在個人 Overleaf 專案編譯並附上真實分享網址
- [ ] 核對指定會議頁數、匿名規則及投稿條件
