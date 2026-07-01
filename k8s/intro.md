# Kubernetes 新手訓練：NPC 直播平台救援任務

你是剛加入團隊的新進 SRE (Site Reliability Engineer, 網站可靠性工程師)

NPC 直播平台最近流量暴增，坤哥的帶貨直播把後端服務打到開始不穩。平台不是單台主機撐起來的，而是跑在 Kubernetes cluster 上；你的任務不是盲目重開機，而是讀懂現場狀態，找出問題，再讓服務恢復穩定。


## 任務目標

- 看懂 Kubernetes 的基本架構
- 確認 cluster 裡有哪些 Node
- 判斷 Node 是否可以接受新的 Pod
- 從 Pod 狀態、事件與 logs 找出異常原因
- 用 Deployment 維持服務副本數

## 遊戲規則

- 先觀察，再下結論
- 只相信 `kubectl` 查到的現場證據
- 不需要背完所有名詞，先掌握能救場的核心路徑
- 看到錯誤訊息不要慌，Kubernetes 通常已經把線索留在狀態、事件或 logs 裡
