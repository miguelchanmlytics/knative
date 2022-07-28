# knative

## components
https://knative.dev/docs/

* Build: 將原始碼建構至容器
* Serving(main): 基于负载自动伸缩，包括在没有负载时缩减到零。允许你为多个修订版本（revision）应用创建流量策略，从而能够通过 URL 轻松路由到目标应用程序。
* Event: 使得生产和消费事件变得容易。抽象出事件源，并允许操作人员使用自己选择的消息传递层。

#[img](https://lh3.googleusercontent.com/k-8ujY6YnFamGKwzt7chkek4so67uH2pK6KXQV311fv-g7kLXOjISQS1TvekJyqRBfPh84QTt1o_J7Pqs_JnHoUT29-TiBcQt5LtsTmFA-uViT2E=w1280)

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

## advantage
* Autoscaling from 0.
* Saving resources and costs(Cloud).





## Cloud Service

|-|AWS|GCP|
|---|---|---|
|CAAS|Fargate|[Cloud Run](https://cloud.google.com/run/?--&gclid=Cj0KCQjwxIOXBhCrARIsAL1QFCZhEeTGAhVhaw0Th6hosSZKHXc5bQeB3xvEJy4YaVl44n5SypUzx9waAvp8EALw_wcB&gclsrc=aw.ds)|
|FAAS|Lambda|Cloud Function|



