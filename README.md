# 我的儀表板

一個單檔、免建置、免 API key 的個人瀏覽器首頁。毛玻璃風格，可切換淺色／深色主題，每天換一張背景照片，把時間、天氣、待辦、匯率、新聞等常用資訊集中在一個畫面。

## 快速開始

不需要安裝任何套件，直接用瀏覽器開啟 `index.html` 即可：

```bash
open index.html          # macOS
```

若瀏覽器限制 `file://` 的網路請求，可改用本機伺服器：

```bash
python3 -m http.server 8000
# 瀏覽 http://localhost:8000
```

也可以把它設成瀏覽器的首頁或新分頁頁面。

## 功能

| 區塊 | 內容 | 資料來源 | 更新頻率 |
| --- | --- | --- | --- |
| 頁首 | 依時段變化的問候語、Google 搜尋、換背景、主題切換（自動／淺色／深色） | — | — |
| 書籤 | 常用網站捷徑，可新增、刪除 | 本機儲存 | — |
| 時間 | 時鐘、日期、農曆、節氣、月相、世界時鐘（東京、倫敦、紐約、舊金山、雪梨、巴黎） | 本機計算 | 每秒 |
| 天氣 | 目前天氣、體感溫度、濕度、風速、AQI、PM2.5、紫外線、24 小時溫度曲線、7 日預報；可搜尋城市或使用目前位置 | [Open-Meteo](https://open-meteo.com/)（天氣、空氣品質、地理編碼） | 15 分鐘 |
| 待辦事項 | 新增、勾選、清除已完成 | 本機儲存 | — |
| 國定假日 | 台灣近期國定假日、年度進度條 | [ruyut/TaiwanCalendar](https://github.com/ruyut/TaiwanCalendar)（經 jsDelivr） | 每天 |
| 地震 | 近 7 天規模 2.5 以上地震 | [USGS](https://earthquake.usgs.gov/) | 5 分鐘 |
| 匯率 | 美元、日圓、歐元、人民幣、港幣、韓元、英鎊兌新台幣 | [ExchangeRate-API](https://www.exchangerate-api.com/)（open.er-api.com） | 1 小時 |
| 加密貨幣 | BTC、ETH、SOL、BNB、XRP、DOGE 的 USDT 價格與 24 小時漲跌 | Binance 公開 API | 1 分鐘 |
| 番茄鐘 | 專注 25／短休 5／長休 15 分鐘，結束時提示音，統計今日完成數 | 本機 | — |
| 地圖 | 以地址、地名或經緯度查詢，可調整縮放，可同步到天氣城市 | Google Maps 內嵌 | — |
| Hacker News | 熱門前 12 則 | [HN Firebase API](https://github.com/HackerNews/API) | 10 分鐘 |
| 維基百科・今日 | 今日特色內容與歷史上的今天（中文無資料時改用英文） | Wikipedia REST API | 1 小時 |
| 背景 | 每天固定一張照片，按「換背景」隨機更換 | [Picsum Photos](https://picsum.photos/) | 每天 |

## 資料儲存

所有個人設定都存放在瀏覽器的 `localStorage`，不會上傳到任何伺服器。換瀏覽器或清除網站資料後會恢復預設值。

| Key | 內容 |
| --- | --- |
| `theme` | 主題（`auto` / `light` / `dark`） |
| `bgSeed` | 當天的背景照片種子 |
| `bookmarks` | 書籤清單 |
| `loc` | 天氣城市（預設台北市） |
| `todos` | 待辦事項 |
| `pomo` | 今日完成的番茄鐘數 |
| `map` | 地圖查詢與縮放等級 |

## 自訂

所有設定都在 `index.html` 內，直接修改即可：

- **配色**：`<style>` 開頭的 `:root` 設計 tokens（淺色與深色各一組）。
- **世界時鐘**：`ZONES` 陣列（名稱 + IANA 時區）。
- **匯率幣別**：`FX` 陣列（幣別代碼、名稱、顯示單位）。
- **加密貨幣**：`COINS` 陣列（代號、名稱）。
- **預設書籤**：`bookmarks` 的預設值。
- **預設城市**：`loc` 的預設值。
- **更新頻率**：檔案最末段「啟動 & 定時更新」中的 `setInterval`。

地圖也可以在瀏覽器 Console 直接操作：

```js
setMap({ q: '台北101', z: 16 })           // 地名 + 縮放等級（1～21）
setMap({ q: '25.0339,121.5645' })        // 經緯度
```

## 注意事項

- 需要網路連線；各區塊獨立載入，單一資料來源失敗不影響其他區塊。
- 部分地區可能無法連線 Binance，加密貨幣區塊會顯示載入失敗。
- 農曆使用瀏覽器內建的 `Intl` 中國曆法；節氣為低精度計算（誤差約 ±1 小時）。
- 「使用目前位置」需要瀏覽器授權定位。
- 版面支援桌機、平板與手機寬度，並尊重系統的「減少動態效果」設定。

## 技術

純 HTML + CSS + 原生 JavaScript，無框架、無建置步驟、無外部 JS 相依。字型使用 Google Fonts 的 Inter，圖示為內嵌 SVG。

---

powered by **Claude** × **William77**
