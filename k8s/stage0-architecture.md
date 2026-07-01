# Stage 0：Kubernetes 架構導覽

## 任務簡報

在進入事故現場前，先建立 Kubernetes 的基本地圖。

Kubernetes 不是一台機器，也不是單純的 container 啟動器。它是一個負責管理 cluster 狀態的平台：你宣告想要的狀態，Kubernetes 持續檢查現況，並把系統拉回正確方向。

## 你現在要懂的事

- `kubectl` 是你和 cluster 溝通的主要工具
- API Server 是 Kubernetes 的入口，所有操作都會先進到這裡
- `etcd` 儲存 cluster 狀態
- Scheduler 決定 Pod 要放到哪個 Node
- Controller 持續比對期望狀態與目前狀態
- kubelet 在 Node 上照顧 Pod，確保 container 被正確啟動

## 架構地圖

```text
你 / kubectl
    |
    v
API Server
    |
    +--> etcd：保存 cluster 狀態
    +--> Scheduler：決定 Pod 放到哪個 Node
    +--> Controller：修正 current state
    |
    v
Node
    |
    +--> kubelet：管理 Node 上的 Pod
    +--> container runtime：啟動 container
    +--> Pod：Kubernetes 最小部署單位
```

## 觀察重點

後面的每一關都會回到這張地圖：

- 查 Node：確認 cluster 有哪些工作現場
- 查 Pod：確認服務實際跑在哪裡
- 查 events / logs：確認 Kubernetes 留下的線索
- 查 Deployment：確認誰在維持服務副本數

## 生圖提示詞

預計檔名：`assets/architecture.png`

```text
Create a clean professional Kubernetes architecture diagram for beginner SRE training. Show a user using kubectl connecting to the API Server. From API Server, show etcd as state storage, Scheduler as pod placement decision maker, and Controller Manager as reconciliation loop. Show two Nodes below, each with kubelet, container runtime, and Pods. Use a modern technical training style, clear labels, simple arrows, no dense text, high readability, 16:9 layout.
```

## 思考一下

如果一個 Pod 壞掉後又自動長回來，是哪一類元件在持續把 current state 修回 desired state？
