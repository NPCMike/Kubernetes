# Stage 2：Node 基本維運

## 任務簡報

直播平台還在跑，但其中一台 worker node 準備維護。

直接拔電源很刺激，但不專業。正確做法是先告訴 Kubernetes：這台 Node 暫時不要再接新的 Pod。

## 你現在要懂的事

- Scheduler 會替新的 Pod 選擇合適的 Node
- `cordon` 會讓 Node 暫停接受新的 Pod
- `uncordon` 會讓 Node 恢復接受新的 Pod
- `SchedulingDisabled` 不等於 Node 壞掉
- 已經在 Node 上執行的 Pod 不會因為 `cordon` 自動被移走

## 開始操作

先確認 Node 狀態：

```bash
kubectl get nodes
```

選一台 worker node，讓它暫停接受新的 Pod：

```bash
kubectl cordon <node-name>
kubectl get nodes
```

維護結束後，讓它回到可排程狀態：

```bash
kubectl uncordon <node-name>
kubectl get nodes
```

## 觀察重點

執行 `cordon` 後，觀察該 Node 的 `STATUS`。

你應該會看到類似：

```text
Ready,SchedulingDisabled
```

這代表 Node 仍然 Ready，但 Scheduler 不會再把新的 Pod 派到這台 Node。

## 卡關提示

如果你不確定哪台是 worker node，回到上一關看 `ROLES` 欄位。

如果你看到 `SchedulingDisabled`，先不要把它當成故障。這通常是人為維護狀態。

## 思考一下

`cordon` 改變的是 Node 的健康狀態，還是改變 Scheduler 是否能把新 Pod 派到那台 Node？
