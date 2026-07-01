# Stage 3：Pod 異常排查

## 任務背景

服務開始不穩，有些 Pod 看起來沒有正常啟動。

## 任務目標

不要只看 `STATUS`，要學會從 describe、logs、events 找原因。

## 排查工具

```bash
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events --sort-by=.lastTimestamp
```

## 思考一下

當 Pod 顯示 `ImagePullBackOff`、`CrashLoopBackOff` 或 `Pending` 時，你會先看哪個資訊？

