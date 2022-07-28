# knative

## components
https://knative.dev/docs/

* Build: 將原始碼建構至容器(faas)
* Serving(main): 基于负载自动伸缩，包括在没有负载时缩减到零。允许你为多个修订版本（revision）应用创建流量策略，从而能够通过 URL 轻松路由到目标应用程序。
* Event: 使得生产和消费事件变得容易。抽象出事件源，并允许操作人员使用自己选择的消息传递层。



### Serving
```
kubectl get deployments -n knative-serving

NAME               READY   UP-TO-DATE   AVAILABLE   AGE

activator          1/1     1            1           5d20h

autoscaler         1/1     1            1           5d20h

controller         1/1     1            1           5d20h

networking-istio   1/1     1            1           2d18h

webhook            1/1     1            1           5d20h
```

[Working process diagram](https://lh3.googleusercontent.com/k-8ujY6YnFamGKwzt7chkek4so67uH2pK6KXQV311fv-g7kLXOjISQS1TvekJyqRBfPh84QTt1o_J7Pqs_JnHoUT29-TiBcQt5LtsTmFA-uViT2E=w1280)

* 應用程序的流量重定向到Activator組件是使用了 Istio 中 VirtualServices 和 DestinationRules 
* Activator 是一个共享组件，其捕获所有到待命 Revision 的流量。当它收到一个到某一待命 Revision 的请求后，它转变 Revision 状态至 Active。然后代理请求至合适的 Pod。
* Autoscaler 收集打到 Revision 并发请求数量的有关信息。每两秒对这些指标进行评估。基于评估的结果，它增加或者减少 Revision 部署的规模。

## CRD
* Service（服务）
* Route（路由）
* Configuration（配置）
* Revision（修订版本）

### Configuration
```
apiVersion: serving.knative.dev/v1
kind: Configuration
metadata:
  name: blue-green-demo
  namespace: default
spec:
  template:
    spec:
      containers:
        - image: gcr.io/knative-samples/knative-route-demo:blue # The URL to the sample app docker image
          env:
            - name: T_VERSION
              value: "blue"

```

### Revision
 * 当您修改 Configuration 的时候（甚至 image 未改變），Knative 会创建一个 Revision。
 * 記錄某一时刻的代码和 Configuration 的快照
 * 是不可變物件，不会被改变和删除


### Route
* 該資源將網路端點映射到一個或多個 revision
```
apiVersion: serving.knative.dev/v1
kind: Route
metadata:
  name: func003
  namespace: default
spec:
  traffic:
    - revisionName: func003-rlddm
      percent: 50
    - revisionName: func003-fzcvh
      percent: 50
```

### Service
* 該資源用來自動管理整個工作負載的生命週期, 一個 service 啟用後會一次建立其他 CRD

[CRD Relation](https://lh5.googleusercontent.com/xyj8XYaAM-tj16RX66oNPVnzPIO2Xm8cJbgue6EpZNzRXVzLsjFXBaePvIR_RfcoJyFuizLR5gcKfcH9k217R2AQyreWBXFC8VU7uo9LOgwxVp65=w1280)


```
kubectl get ksvc

NAME            URL                                                   LATESTCREATED         LATESTREADY           READY   REASON

func002         http://func002.default.162.222.182.209.xip.io         func002-2bzdc         func002-2bzdc         True    

kubectl get revision

NAME                     CONFIG NAME        K8S SERVICE NAME      GENERATION   READY   REASON

func002-2bzdc            func002            func002-2bzdc         1            True    

kubectl get configuration

NAME               LATESTCREATED            LATESTREADY           READY   REASON

func002            func002-2bzdc            func002-2bzdc         True   

kubectl get route

NAME              URL                                                     READY   REASON

func002           http://func002.default.162.222.182.209.xip.io           True   

 
```

## [Event](https://lh5.googleusercontent.com/mASinbXtseQ5GFHt9mS9tKas9q8YPeGduR3FMeOTZfnwpanAsSNMzGxlLnGdkwcPrdR_xg=w1280)


## advantage
* Autoscaling from 0.
* Saving resources and costs(Cloud).

## disadvantage
* cold start time


## Cloud Service

|-|AWS|GCP|
|---|---|---|
|CAAS|Fargate|[Cloud Run](https://cloud.google.com/run/?--&gclid=Cj0KCQjwxIOXBhCrARIsAL1QFCZhEeTGAhVhaw0Th6hosSZKHXc5bQeB3xvEJy4YaVl44n5SypUzx9waAvp8EALw_wcB&gclsrc=aw.ds)|
|FAAS|Lambda|Cloud Function|



