# Vue筆記
## 1.安裝環境
1. 安裝node  
1. 安裝npm(Node Package Manager,安裝node後會順便安裝)  
1. 安裝vue-cli(命令提示字元工具)
## 2.建立vue專案
環境建立好之後在命令提示字元輸入
```
vue init webpack [project name]
```
webpack是vue提供的樣板名稱,可以用以下命令查看有哪些樣板
```
vue list
```
## 3.安裝依賴項
到專案建置好的目錄
```
cd [project name]
npm install
```
## 4.啟動
```
npm run dev
```
## 5.結束
Ctrl-C

## 6.Demo (heroku)
- https://vue-todo-list500.herokuapp.com/#/Todolist

## 7.備註
部屬到heroku時要注意heroku安裝的node,npm版本(從package.json中調整)


## 參考資料
- https://github.com/danygitgit/document-library/tree/master/JavaScript-library/Vue
- https://v3.cn.vuejs.org/
- https://ithelp.ithome.com.tw/articles/10193564
- https://medium.com/unalai/%E8%AA%8D%E8%AD%98-heroku-%E5%AD%B8%E7%BF%92%E5%B0%87-vue-%E5%B0%88%E6%A1%88%E9%83%A8%E7%BD%B2%E8%87%B3heroku-4f5d8bd9b8e2