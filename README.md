# FM Radio STT-TTS-with-AI
將無線電 Radio 的語音轉文字 (STT: speech to text), 然後利用 AI 對話機器人回覆後, 再將其文字轉語音 (TSS: text to speec), 並且透過電路發送回無線電上.

前言: ..

採用邊緣運算(Edge AI)將無線電的語音轉文字(STT), 若能套上大語言模型(LLM), 並且加上提示語模版(Prompt)展現出比傳統單純語音辨識更強大的功能, 最後將其輸出結果的文字轉語音, 相對是簡單很多, 當然這中間全部的文字都會透過 Line 通訊軟體, 以及 MQTT + NodeRed 實現在手機或者網頁上之外, 也同步會利用電路將語音發送回到無線電上.

這裡有一個很重要的問題是 Edge AI 需要靠aa

