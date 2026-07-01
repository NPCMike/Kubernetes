# Stage 3：Pod 異常排查

## 任務簡報

直播間開始卡，客服頻道也開始熱鬧。

不要先猜是哪個人改壞，也不要先重啟整個世界。SRE 的第一步是收集證據：Pod 狀態、事件、logs，通常已經把問題方向寫出來了。

## 你現在要懂的事

- `STATUS` 是入口，不是結論
- `describe pod` 可以看到事件、排程、image pull、restart 等線索
- `logs` 用來看 container 內部輸出
- `events` 可以從 cluster 角度看最近發生的事
- 不同錯誤狀態代表不同排查方向

## 開始操作

先看所有 namespace 裡的 Pod：

```bash
kubectl get pods -A
```

挑一個異常 Pod 深入看細節：

```bash
kubectl describe pod <pod-name> -n <namespace>
```

查看 container logs：

```bash
kubectl logs <pod-name> -n <namespace>
```

查看最近事件：

```bash
kubectl get events -A --sort-by=.lastTimestamp
```

## 觀察重點

看到這些狀態時，先往對應方向查：

- `ImagePullBackOff`：image 名稱、tag、registry 權限或網路問題
- `CrashLoopBackOff`：container 有啟動，但程式反覆崩潰；優先看 logs
- `Pending`：Pod 還沒被排上 Node；優先看 describe 裡的 scheduling events
- `Running` 但服務不正常：狀態不一定代表應用程式真的健康，繼續看 logs 與 readiness 設定

## 卡關提示

`kubectl describe pod` 的最下方通常有 `Events`。

如果你只看 `STATUS`，你只知道「它壞了」。如果你看 `Events`，你會知道「它為什麼壞」。

## 生圖提示詞

預計檔名：`assets/pod-debug-decision-tree.png`

```text
Create a professional Kubernetes pod troubleshooting decision tree for beginner SRE training. Start with "kubectl get pods -A" then branch by Pod status: ImagePullBackOff, CrashLoopBackOff, Pending, Running but unhealthy. For each branch, show the next command: describe pod, logs, events, check image, check scheduling, check application logs. Use clear boxes and arrows, concise labels, modern technical diagram style, high readability, 16:9 layout.
```

## 思考一下

當 Pod 顯示 `CrashLoopBackOff` 時，你會先看 `describe`、`logs`，還是 `events`？你期待在哪裡看到最接近應用程式錯誤的線索？
