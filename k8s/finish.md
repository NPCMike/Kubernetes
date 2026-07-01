# Mission Complete：直播平台恢復穩定

事故現場先收工。

你已經完成 Kubernetes 新手 SRE 的第一輪任務：不是背完所有名詞，而是知道進入 cluster 後要先看什麼、壞掉時去哪裡找線索，以及為什麼 Deployment 能把服務補回來。

## 你現在能做到的事

- 用 `kubectl get nodes` 判斷 cluster 裡有哪些 Node
- 看懂 control plane 與 worker node 的角色差異
- 用 `cordon` / `uncordon` 控制 Node 是否接受新的 Pod
- 用 `describe`、`logs`、`events` 排查 Pod 異常
- 用 Deployment 維持服務副本數，理解 Pod 被刪除後為什麼會自動補回

## 最後確認

Kubernetes 的核心不是「幫你跑一個 container」。

它真正做的是：持續檢查系統現況，並把現況拉回你宣告的期望狀態。
