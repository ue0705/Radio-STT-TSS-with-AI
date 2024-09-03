# FM Radio STT-TTS-with-AI
將無線電 Radio 的語音轉文字 (STT: speech to text), 然後利用 AI 對話機器人回覆後, 再將其文字轉語音 (TSS: text to speec), 並且透過電路發送回無線電上.

前言:
2024年4月13日台灣有發生大地震, 加上台灣一直有戰爭的陰影, 所以手上有買兩隻俗稱火腿族的手持無線電收發器, 以防止緊急情況時可以使用, 後來發現有一個頻道會常常報出家裡附近地點的情況, 例如違規停車/吵架/路倒/車禍/火災/噪音通報等等, 但我總不可能24小時都在監聽這個聲音, 網路上之前有一個工程師很猛, 直接把它接上 Youtube 直播, 這樣就可以隨時回放並且實現遠端聆聽, 結果被送辦.
https://www.119.gov.taipei/News_Content.aspx?n=0EDB9C505FE8B199&sms=18AF49168D1E8B97&s=91A7E793691A6D26

當然我們不會這麼白目, 但又想隨時可以知道內容, 於是想到利用語音辨識後轉傳到 Line 上面, 但無線電的聲音實在是太糟糕, 加上內容斷斷續續, 後來利用 ChatGPT LLM 的大語言模型來判斷, 發現準確非常多, 我只要看手機 Line 就可以知道內容了, 只是需要 Token = Money, 就這樣系統開始運行後, 就看到家裡附近真的很糟糕, 常常出很多事情, 但久了發現這只是日常, 應該全台灣各地都是樣子的. XD

結果 2024年6月, 新竹發生晴空匯大火, 有兩名消防員在無線電上面喊了5次 Mayday 都沒有人聽到, 於是錯過了黃金救援時間, 我當下看到這個新聞甚是難過, 但忽然想起我的無線電轉成文字後, 要攔截關鍵字這簡直是非常容易的, 於是稍微修改一下 python 代碼, 然後用無線電喊了一些關鍵字, 其實是可以輕易辨識的, 所以當下就在想有沒有可能利用這個裝置, 無償提供給消防局使用.
https://www.cna.com.tw/news/aloc/202406040436.aspx
![image](https://github.com/user-attachments/assets/728e6471-6475-409f-bfaf-7f53ef31a1d8)

在 Mediatek 內部除了有 Davinchi 之外, 還有一個 Breeze AI 對外的開放的公版 https://huggingface.co/MediaTek-Research, 所以除了 ChatGPT(OpenAI) / Azure(Microsoft) / Gemini(Google) / llama(FB) 還有自家產品, 無論是何種都是需要雲端運算 = Money = Tokey, 加上4G上網與運算平台, 費用的問題其實還好解決, 但資源調度與曝光度其實也相對的重要, 於是我選擇了聯發科基金會的智在家鄉, 其實滿符合他的宗旨, 能夠利用科技回饋鄉里, 並且創造出價值, 這個結合AI與創新的應用, 協助火場發生時的處理等等, 應該是個不錯的想法, 除了消防也可應用在警方攻堅/山區救援/戰場指揮等特殊情況下的無線電使用都可以, 至於為什麼這些場景不使用手機系統而使用無線電, 這個問題就不在這裡探討, 我們只是將無線電結合網路與AI, 就可以發揮他更大的功能.

回到智在家鄉, 由於是公司內部人員參加, 所以並沒有獎金, 純粹是志(智)在參加不在得獎, 但利用這個參加的名義而非私人項目, 其實也助於Project的進展, 我其實可以在公司將近萬人的工程師中, 尋找可以協助發展的夥伴, 這個功能需要採用邊緣運算(Edge AI)將無線電的語音轉文字(STT), 若能套上大語言模型(LLM), 並且加上提示語模版(Prompt), 展現出比傳統單純語音辨識更強大的功能, 最後將其輸出結果的文字轉語音, 這相對是簡單很多, 當然這中間全部的文字都會透過 Line 通訊軟體, 以及 MQTT + NodeRed 實現在手機或者網頁上之外, 也同步會利用電路將語音發送回到無線電上, 用以達成.
1. 將無線電的語音加上大語言模型轉成文字後, 推送到 Line or Web, 讓群內知道千里之外的無線電通訊內容.
2. 可利用 Line or Web 上輸入文字, 系統會將文字轉語音後發送回無線電上, 實現千里之外與無線電互相通訊.
3. 當 Line 上面呼叫特定關鍵字(AI Name)，則會喚醒 AI 助理, 並且預設 Prompt 協助當時情況輔助您排除困難.
4. 當問文字內容有特定緊急呼叫關鍵字, 會觸發額外的報警系統, 避免因誤判或者疏漏造成遺憾.

這裡有一個很重要的問題是 Edge AI 需要靠單板電腦SBC(Single-board Computer)，來實現 低功耗/高算力/可移動上網/低成本/開放資源充足 之功能, 但這魚與熊掌不可兼得的情況下, Intel/AMD x86平台不可行, NVIDIA Jetson 算力足夠但成本過高, ESP32/Android 雖然便宜但太過簡易, 基本的 python 很難運作, 最後選擇了 Raspberry Pi, 雖然算力未必是最高, 但可透過雲端補足, 自身當作 Edge 應可滿足多數使用, 上網靠4G網卡, 電力靠 18650電池底座約可運作8hr, 至於手機類型的SBC則是之後可以考慮的平台.

--------------------------
上面主要描述前言, 這裡開始要講述整個硬體與軟體的架構.

**硬體部分主要是:**
SBC:Raspberry Pi, Display:OLED 128*64, USB dongle: Audio codec (因為 Pi 沒有 audio in/out), Buzzer(option), LED RGB(option), UPS HAT(option), 上述硬體著重在便宜省電.

**軟體部分比較複雜:**
OLED Driver: 一個 python 每秒把時間/溫度/記憶體使用/CPU使用等丟在螢幕上, 最重要的是顯示目前 IP addr, 這個沒連上網時候會顯示空白, 有上網時會顯示 local ip 非常實用.
DuckDNS: 一個 shell 每三分鐘把 Group/Local IP/Temp/Disk spcae 丟到 duckdns 上面, 因為是 free 所以只能註冊五個 device, 但也夠用了, 後來有 MQTT+NodeRed, 其實這個功能就弱化了, 但不耗資源就不移除他了.
FRP: fast reverse proxy, 一個 github 上的上的開源碼 https://github.com/fatedier/frp, 因為裝置在連上網路後, 例如是熱點分享/防火牆後面等等, 你不可能開對外 port 去連他, 所以利用 FRP 穿透方式, 這樣只要裝置上網, 我都可以反向 SSH 他, 並且遠端控制.
Pi status monitor: 用 python 將目前系統的全部狀態即上面的 duckdns 功能之外, 還提供 UPS 電池容量, 以及遠端 GPIO 控制(即 IoT 功能), 遠端命令等等, 使用 MQTT 傳送, 然後 NodeRed 整合控制介面, 其實外面有現成的軟體可以 monitor 出目前系統的狀態也很詳細, 但缺少我要的 GPIO / UPS_HAT / Command 控制等功能, 所以還是用目前的方案較合適.
UPS HAT drvier: 他的範例是 python to print log, 改成 python to MQTT, 再利用 NodeRed 把狀態畫出來.

**Server:** 這個我一度要上去買雲端, 但會被限制 CPU 使用量, 硬碟容量, 網路流量, 而且還要付費, 後來我利用 QNAP NAS + Container Station(Dockert) 來完美解決
FRP: 他的原理其實很簡單, 就是 client / host 都連上來, 他負責把網路封包 proxy 轉發, 所以你傳 100MB 檔案, 就會通通吃他的頻寬, 還好 NAS 本身就在雙向1Gbps固網上, 所以就沒有這個困擾, 連我陽明山上的 Mac mini M2 本來也被防火牆問題困擾, 遠端遙控需透過難用的 Teamviewer, 後來有了 FRP 就可以使用內建的 share(VNC) 來遠端控制, 連我女兒的 Windows 電腦, 我也埋了 FRP 來偷偷遠端控制.
MQTT: 以前常聽到但沒使用過, 後來問 ChatGPT 我如果有兩台機器甚至是微處理器(ESP32), 想要共享memory或者互相通訊, 甚至於我想跨平台(Mac/Windos/Pi/ESP32), 最後發現了 MQTT, 於是把他架在 NAS Docker 上, 一旦 Server 掛點, 則無法提供此功能, 故之後還是會考慮把這部分購買雲端伺服器.
Node-Red: 當資料可以共享之外, 也需要一個 Web 來整理這些資料, 於是 ChatGPT 一樣推薦 NodeRed, 於是一樣架設在 NAS Docker 上, 所以目前有一個 Web 控制介面, 可以綜觀全部的 Pi 狀態, 與目前 Radio STT 全部的訊息, 緊急情況的控制等等.

