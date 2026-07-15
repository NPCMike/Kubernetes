# Mission Complete：救援任務回報

## 這一關的情境

事故現場先收工。你還不是 Kubernetes 大師，但你已經完成新手 SRE 的第一輪救援任務，也完成第二輪基礎維運任務。

你沒有靠猜，也沒有靠亂重開。你做的是：

1. 先看 Kubernetes 地圖。
2. 再點名 Node。
3. 再理解維護 Node 時怎麼暫停接新 Pod。
4. 接著用狀態、事件、logs 排查 Pod。
5. 看懂 Deployment 為什麼會把 Pod 補回來。
6. 用 Namespace 把資源放到正確區域。
7. 用 Service 替會變動的 Pod 提供穩定入口。
8. 用 ConfigMap 和 Secret 分開管理設定與敏感資訊。

## 你現在能做到的事

你現在可以用 `kubectl get nodes` 看 cluster 裡有哪些 Node。

你可以從 `ROLES` 分辨哪台比較像 control plane，哪台比較像 worker node。

你知道 `cordon` 是讓 Node 暫停接受新的 Pod，`uncordon` 是恢復接受新的 Pod。

你知道看到 Pod 異常時，不能只盯著 `STATUS`。你會接著看 `describe`、`events` 和 `logs`。

你知道 Deployment 的重點是維持副本數，也就是把 current state 拉回 desired state。

你知道 Namespace 是資源分區，不是新的機器。

你知道 Service 是穩定入口，靠 selector 和 label 找到背後的 Pod。

你知道 ConfigMap 適合放非敏感設定，Secret 才適合放 token、密碼和憑證。

## 最後總整理

```text
查現場
  kubectl get nodes
  kubectl get pods -A

找線索
  kubectl describe pod ...
  kubectl logs ...
  kubectl get events ...

維持狀態
  Deployment + replicas
  desired state vs current state

分區與入口
  Namespace + -n + -A
  Service + selector + endpoints

設定與敏感資訊
  ConfigMap = 非敏感設定
  Secret = 敏感資訊
```

## 小任務：確認你真的懂

請用自己的話回答這五題：

1. Node 和 Pod 差在哪裡？
2. 看到 `CrashLoopBackOff` 時，為什麼要看 logs？
3. 為什麼刪掉 Deployment 管理的 Pod 後，Kubernetes 會補新的 Pod？
4. `kubectl get pods -n npc-live` 和 `kubectl get pods -A` 差在哪裡？
5. ConfigMap 和 Secret 最大差別是什麼？

如果你能回答這五題，你已經抓到這堂課最重要的骨架了。

## 收尾句

Kubernetes 入門不是先背百科全書。

它的第一步是學會看現場：

> 哪裡在跑？哪裡異常？資源在哪一區？流量從哪個入口進來？設定和秘密放在哪裡？Kubernetes 想維持什麼狀態？現在狀態和目標差在哪裡？

只要你開始用這個方式看問題，就已經從「看到錯誤就慌」往「看線索做判斷」前進一大步。
