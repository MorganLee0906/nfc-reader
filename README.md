# NFC Mini 卡片管理(網頁版)

用 Web Bluetooth 直接控制讀卡機的單頁網頁,取代微信小程式。純前端、無伺服器、
資料存在瀏覽器本機,適合個人卡片管理與備份。

## 支援環境

| 環境 | 可否 |
|---|---|
| 桌機 Chrome / Edge(含 macOS) | ✅ |
| Android Chrome | ✅ |
| Safari(桌機或 iOS)、任何 iOS 瀏覽器 | ❌ Web Bluetooth 不支援 |

**必須從 `https://` 或 `http://localhost` 開啟** —— 直接雙擊 `index.html`(file://)不行。

## 部署方式

### 線上版(主要)——GitHub Pages
**https://morganlee0906.github.io/nfc-reader/**

Pages 本身就是 https,是「安全內容」,Web Bluetooth 直接可用,不必設定任何東西。
Mac 和 Android 的 Chrome 打開網址就能連讀卡機。改 `index.html` push 到 `master`
分支後,Pages 會自動重新部署。

### Mac 本機(離線 / 開發用)
```bash
cd <webapp 目錄>
./run.sh                    # 起 http://localhost:8000
```
`localhost` 也是安全內容,藍牙一樣可用。

## 功能

四個分頁,給一般人用的介面:

- **簽到**:按「開始簽到」進入靠卡模式,刷卡即顯示該人姓名/備註並記錄時間;未建檔的卡會提示。
  今日紀錄可即時查看、匯出 CSV。
- **名冊**:建檔的卡片清單,可改姓名/備註、搜尋、刪除、匯出/匯入 JSON。
- **新增卡片**:後台刷卡建檔 —— 讀卡後填姓名/備註存入名冊。
- **進階**(功能收在可展開選單內):完整讀取、寫門禁卡、複製 UID 到 magic 卡、寫單一 block、連線紀錄。

資料存在瀏覽器 localStorage:名冊 `nfcmini_cards`、簽到紀錄 `nfcmini_checkins`。

## 注意

- 寫卡不可復原,且需可寫入的卡(ID 卡用 T5577;IC 改 UID 用 CUID/UID magic 卡)。
- 寫入指令的封包格式與擷取到的原廠位元組逐一比對過(見上層 `capture-2-att-all.txt`);
  寫 UID 時 block0 的位元組順序會自動反序,與原廠一致。
- 但每個寫入指令目前只有一個原廠樣本,若遇到不同卡種行為不同,屬正常,先在可犧牲的卡上試。
- 資料只存在瀏覽器,清瀏覽器資料會一起清掉 —— 定期匯出 JSON。
