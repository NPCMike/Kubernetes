# K8S Killercoda 新手闖關教材設計

## 目標

把 Kubernetes 新手訓練做成 Killercoda 引導式闖關教材。學員不是來背名詞，而是扮演新進 SRE，接手 NPC 直播平台事故現場，用少量提示和實作指令一步步理解 cluster、node、pod debug 與 deployment。

## 教材調性

採用內部訓練玩梗版。保留 NPC 直播平台、坤哥、流量暴增、SRE 救火等主線，讓教材有記憶點，但每一頁仍以任務和操作為主，不讓劇情壓過學習目標。

Killercoda 頁面要短。每一關只放關鍵概念、必要指令、觀察方向和卡關提示。較長的原理說明交給講師口頭補充或課後資料。

## 章節架構

### Intro：任務背景

建立情境：NPC 直播平台因流量暴增導致服務不穩，學員以新進 SRE 身分進入事故現場。

目標只列四件事：

- 看懂 Kubernetes 架構
- 確認 Node 狀態
- 找出 Pod 壞掉的原因
- 用 Deployment 修復與擴充服務

### Stage 0：Kubernetes 架構短講

用 5-10 分鐘建立心智模型：Kubernetes 是「宣告期望狀態，讓系統持續修正現況」的平台。

只介紹必要架構：

- `kubectl`：學員下指令的入口
- API Server：cluster 的主要入口
- `etcd`：記住 cluster 狀態
- Scheduler：決定 Pod 要放到哪個 Node
- Controller：持續修正現況
- Node：真正執行 workload 的機器
- kubelet：Node 上負責照顧 Pod 的 agent
- Pod：Kubernetes 最小部署單位

### Stage 1：認識你的 Cluster

學員用 `kubectl get nodes` 和 `kubectl get nodes -o wide` 觀察 cluster。重點是分辨 control plane 與 worker node，理解自己管理的是 cluster，不是單台 server。

### Stage 2：Node 基本維運

學員用 `cordon` 和 `uncordon` 練習維護節點。重點是理解 `SchedulingDisabled` 不是 Node 壞掉，而是暫停把新的 Pod 排到那台 Node。

### Stage 3：Pod 異常排查

學員用 `get pods`、`describe pod`、`logs`、`events` 查線索。重點不是背錯誤名詞，而是知道看到 `ImagePullBackOff`、`CrashLoopBackOff`、`Pending` 時下一步該去哪裡找原因。

### Stage 4：Deployment 修復與擴充

學員建立 Deployment、觀察 replicas、刪掉 Pod，確認 Kubernetes 會自動補回。重點收斂到 desired state 與 current state：Deployment 不是單純建立 Pod，而是維持「希望有幾份服務存在」。

## 每頁內容格式

每個 stage 優先使用這個結構：

- 任務簡報：2-4 句劇情帶入
- 你現在要懂的事：3-5 個關鍵點
- 開始操作：少量 `kubectl` 指令
- 觀察重點：學員要看哪個欄位或狀態
- 卡關提示：給方向，不直接暴雷
- 思考一下：用一題收斂概念

## 圖片 Prompt 原則

不是每頁都要有圖。只有當圖片能明顯降低理解成本時，才放「生圖提示詞」區塊。

優先放圖片提示詞的位置：

- Stage 0：Kubernetes 架構圖，因為新手需要先建立 cluster 心智模型
- Stage 3：Pod debug 決策樹，因為異常排查路徑容易混在一起
- Stage 4：Deployment 自動補 Pod 流程圖，因為 desired state 與 current state 適合視覺化

可不放圖片的位置：

- Intro：除非要做封面圖，不然劇情文字已足夠
- Stage 1：`kubectl get nodes` 輸出本身就是主要教材
- Stage 2：流程很短，用文字與指令即可

圖片提示詞要寫成可直接拿去生圖的描述，並標明預計檔名，例如 `assets/k8s-architecture.png`。Markdown 先不要引用不存在的圖片，避免 Killercoda 顯示破圖。

## 驗收標準

- `k8s/index.json` 可以被 JSON parser 正確解析
- 每個 step 檔案存在，且文字量適合 Killercoda 任務面板
- 教材保留內部玩梗語氣，但每關仍有清楚學習目標
- 圖片 prompt 只出現在必要位置
- 不引入 Service、Ingress、CNI、CSI、RBAC、Admission Webhook 等超出本次新手闖關範圍的主題
