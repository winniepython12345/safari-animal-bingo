# Singita Safari Bingo — LINE 小工具

25 種 safari 動物隨機賓果卡,自動帶入 LINE 顯示名稱與建立日期,可分享圖片或邀請朋友到群組一起玩。

## 檔案說明

- `index.html`:整個小工具只有這一個檔案(HTML/CSS/JS 都寫在裡面),部署時整個資料夾丟上主機即可。

## 目前狀態

程式已經可以在瀏覽器直接打開測試(雙擊 `index.html`),此時「姓名」欄需要自己手動輸入,因為還沒接上真正的 LINE 帳號資訊。要讓它能在 LINE 群組裡使用、自動帶入 LINE 名稱,需要完成下面兩步:**申請 LIFF、部署到有 HTTPS 的網址**。

---

## 步驟一:建立 LINE LIFF(約 10 分鐘,全部免費)

1. 前往 [LINE Developers Console](https://developers.line.biz/console/),用你自己的 LINE 帳號登入。
2. 建立一個 **Provider**(如果還沒有的話),名稱隨意,例如「我的小工具」。
3. 在該 Provider 下建立一個新 **Channel**,類型選 **LINE Login**。
4. 進入剛建立的 Channel,找到上方 **LIFF** 分頁,點「Add LIFF app」。
   - **LIFF app name**:Singita Safari Bingo
   - **Size**:選 `Full`(賓果卡格子較高,滿版顯示效果最好)
   - **Endpoint URL**:先隨便填 `https://example.com`,等步驟二部署好網址後再回來改成正式網址
   - **Scope**:勾選 `profile`(用來抓 LINE 顯示名稱)
   - **Share target picker**:打勾啟用(「邀請朋友一起玩」按鈕需要用到)
5. 建立完成後,會拿到一組 **LIFF ID**,格式類似 `1234567890-AbCdEfGh`,先複製起來。

---

## 步驟二:部署網頁(免費、免安裝任何工具)

最簡單的方式是用 **Netlify Drop**(不需要註冊帳號也能先試用,想要網址長期有效建議申請一個免費帳號):

1. 開啟 https://app.netlify.com/drop
2. 把整個 `webapp` 資料夾(包含 `index.html`)直接拖進網頁裡
3. 幾秒後會拿到一個網址,例如 `https://xxxxx.netlify.app`
4. 這就是你的正式網址,把它記下來

（如果你比較熟悉 GitHub,也可以用 GitHub Pages / Vercel 部署,效果相同,原理都是把這個資料夾放到一個公開 HTTPS 網址上。）

---

## 步驟三:把兩邊串起來

1. 打開 `index.html`,找到最上面這一行:
   ```js
   const LIFF_ID = "YOUR_LIFF_ID_HERE";
   ```
   換成步驟一拿到的 LIFF ID,例如:
   ```js
   const LIFF_ID = "1234567890-AbCdEfGh";
   ```
2. 存檔後,把更新後的資料夾重新拖到 Netlify Drop 覆蓋部署(或用 GitHub/Vercel 重新推送)。
3. 回到 LINE Developers Console 的 LIFF 設定頁,把 **Endpoint URL** 改成步驟二拿到的正式網址(例如 `https://xxxxx.netlify.app`)。
4. 在瀏覽器打開這個網址:`https://liff.line.me/你的LIFF ID`,例如 `https://liff.line.me/1234567890-AbCdEfGh`,這個網址就是你要分享到 LINE 群組的連結。

---

## 怎麼在群組使用

把 `https://liff.line.me/你的LIFF ID` 這個連結貼到 LINE 群組裡(可以釘選置頂方便大家找到)。群組成員點開後:

- 會在 LINE 內建瀏覽器開啟,自動帶入自己的 LINE 顯示名稱與當天日期
- 產生專屬的 25 格動物賓果卡,可以直接點格子打勾
- 按「🔀 重新產生」可以換一組新的隨機排列
- 每一格左上角有 📷 按鈕,可以拍照或從相簿選圖上傳,上傳後照片會直接覆蓋掉該格的 emoji 圖示、並自動打勾;旁邊會出現 ✕ 按鈕,按了可以移除照片、換回原本圖示
- 按「📸 分享 / 下載圖片」會把卡片(含上傳的照片)存成一張完整圖片,手機上通常會直接跳出分享選單,選 LINE 就能傳回聊天室
- 按「👥 邀請朋友一起玩」會叫出 LINE 原生的分享視窗,直接把連結送到指定的聊天室,不用手動複製貼上

> 上傳的照片只存在瀏覽器當下畫面(不會上傳到任何伺服器),按「重新產生」洗牌後會清空,適合每次遊戲重新拍照留念。

## 之後想調整的地方

- **動物清單/圖示**:在 `index.html` 裡搜尋 `const ANIMALS`,是一個陣列,每筆有中文名、英文名、emoji 圖示,想換動物或圖示直接改這裡。
- **視覺風格**(顏色、字體、邊框)集中在 `<style>` 區塊最上面的 `:root` 變數,改色碼就能整體套用。
- 目前動物格子用的是 emoji 圖示(非真實照片),因為原始參考圖是有版權的商業攝影素材,不能直接放進工具裡。如果你有取得授權的動物照片,可以把 `.cell` 內的 emoji 換成 `<img>` 標籤即可。
