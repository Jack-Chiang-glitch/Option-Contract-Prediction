# Option-Contract-Prediction <br> (Provide both Chinese and English versions of README.md.)


資料前處理 : <br>

從Alpha Vantage API爬取歷史選擇權合約，會有數個不同的欄位，有contractID、symbol、expiration、strike、type、last<br>
mark、bid、bid_size、ask、ask_size、volume、open_interest、date、implied_volatility、delta、gamma、theta、vega、rho<br>
存入SQLite資料庫裡面，為了將資料處理成可以放入模型的張量，做以下處理<br><br>

(1)先將距離到期日的時間換算成年(時間價值)<br>
(2)將call/put型態變成one-hot encoding<br>
(3)bid、ask都除以strike，價格正規化，否則各個股票價格相異過大<br>
(4)bid_size、ask_size、volume、open_interest、都換算成當日的百分比，進行正規化<br>
(5)最後將strike除以股價，正規化<br>
(6)所有欄位各自對合約標準化，讓模型比較好學習輸入的向量<br><br>

訓練期資料 : 2015/01/01到2021/12/31，測試期資料 : 2022/01/01到2024/12/31

由於每日的合約數是會變動的，加上彼此之間沒有先後順序，因此用set transformer是合理的選擇<br>
因為transformer具有處理會變動長度的輸入，set transformer的架構可以應付無順序性<br>
![image](https://github.com/user-attachments/assets/824a7be6-f619-4a38-8722-43d58294883e)



Data Preprocessing:<br>
Historical option contract data is retrieved from the Alpha Vantage API, which includes multiple fields such as:<br>
contractID, symbol, expiration, strike, type, last, mark, bid, bid_size, ask, ask_size, volume, open_interest, date, implied_volatility, delta, gamma, theta, vega, and rho.<br>
The data is then stored in an SQLite database. To transform the data into tensors suitable for model training, the following preprocessing steps are applied:<br>

Convert time to expiration into years (time value).<br>
Encode call/put option type using one-hot encoding.<br>
Normalize bid and ask prices by dividing them by the strike price to prevent large discrepancies across different stock prices.<br>
Normalize bid_size, ask_size, volume, and open_interest by converting them into daily percentage values.<br>
Normalize strike price by dividing it by the stock price.<br>
Standardize each field per contract to improve model learning efficiency.<br>
Training & Testing Data:<br>
Training period: 2015/01/01 – 2021/12/31<br>
Testing period: 2022/01/01 – 2024/12/31<br>
Since the number of contracts varies daily, and there is no inherent sequential order among them, using a Set Transformer is a reasonable choice.<br>
Unlike traditional models, Transformers can handle variable-length inputs, and the Set Transformer architecture is well-suited for unordered data structures.<br>
