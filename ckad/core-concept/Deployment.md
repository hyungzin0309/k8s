## Deployment

> Deployment 는 pod 의 replica 를 유지하고 업데이트 방식을 설정하는 등 pod 를 일괄적으로 관리 및 제어하는 여러 기능을 제공한다.  

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
  labels:
    app: example
spec:
  replicas: 3
  selector:
    matchLabels:
      app: example
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1 # 한 번에 내릴 수 있는 구 버전 pod 수
      maxSurge: 1 # rollout 동안 원래 replica 보다 더 생성할 수 있는 pod 수
  minReadySeconds: 5
  revisionHistoryLimit: 10 # 롤백 시 참조할 수 있는 이전 버전의 수를
  paused: false
  progressDeadlineSeconds: 600 # 롤아웃 최대 시간 - 넘으면 오류로 간주
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: example/image
        ports:
        - containerPort: 80

```

### 주의점

- selector 에서 관리대상이 되는 pod의 label 지정 시, 기존에 이미 존재하는 pod의 라벨과 중복되지 않도록 하는 것이 좋다. 어떠한 side effect가 발생할지 모르기 때문에.

---

## (+) ReplicaSet

> Replicaset 도 파드의 replica 를 관리하기 위해 사용되며 yaml 작성도 deployment 와 유사하다.
> <br> 단, Replicaset 은 파드의 복제본을 관리하는 기능만 제공하기 때문에, 해당 기능이 포함되었으며 확장된 기능을 포함한 deployment 를 사용하는 것이 일반적이다.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: example-replicaset
  labels:
    app: example
spec:
  replicas: 3
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: example/image
```