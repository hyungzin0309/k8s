# Pod
> Pod 는 쿠버네티스에서 가장 작은 배포 가능한 단위이며 쿠버네티스 클러스터에서 어플리케이션의 배포, 스케일링 및 관리를 용이하게 하는 핵심 개념이다.
> <br>하나의 Pod 는 하나 이상의 컨테이너를 포함할 수 있으며, 이러한 컨테이너들은 서로 밀접하게 연관되어 있고, 공통된 리소스와 의존성을 공유한다. 각 Pod 는 고유한 IP 주소를 가지며, 여러 컨테이너가 동일한 네트워크와 스토리지 리소스를 공유한다.

Pod 의 주요 특징은 다음과 같다.

1. **컨테이너 그룹핑**
<br> 한 Pod 안에 있는 컨테이너들은 서로 밀접한 관계를 가지고, 같은 작업을 수행하기 위해 함께 배치된다. 예를 들어, 하나의 애플리케이션 서버 컨테이너와 관련 데이터베이스 서버 컨테이너가 하나의 Pod 안에 있을 수 있다.
<br> 그러나 일반적인 환경에서는 하나의 pod 안에 단일 컨테이너를 운영한다.


2. **공유 리소스와 의존성**
<br> Pod 내의 컨테이너들은 네트워크, 스토리지 등의 리소스를 공유한다. 이것은 컨테이너들이 서로 통신하기 쉽고, 데이터를 공유할 수 있게 해준다.


3. **임시성(Temporality)**
<br> Pod 는 임시적인 성격을 가지고 있다. 즉, Pod 가 삭제되면 그 안의 모든 컨테이너도 함께 삭제된다. 이러한 특성은 쿠버네티스가 고가용성과 스케일링을 관리하는 데 도움이 된다.


4. **독립적인 배치**
<br> 각 Pod 는 독립적으로 배치되며, 자체 IP 주소와 호스트 이름을 가진다. 이를 통해 네트워킹과 리소스 할당이 간소화된다.


5. **라이프사이클 관리**
<br> 쿠버네티스는 Pod 의 라이프사이클을 관리한다. 이는 Pod 의 생성, 스케일링, 업데이트 및 삭제를 포함한다.

## 생성

### 명령어

```
kubectl run example-pod --image=myapp:latest --port=80 --env="ENV_VAR=example-value" --requests='cpu=250m,memory=64Mi' --limits='cpu=500m,memory=128Mi' --labels="app=myapp,role=frontend" --annotations="example.com/annotation=example-value" --restart=Always
```

### yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
  labels:
    app: myapp
  annotations:
    example.com/annotation: "example-value"
spec:
  containers:
  - name: app-container
    image: myapp:latest
    ports:
    - containerPort: 80
    env:
    - name: ENV_VAR
      value: "example-value"  
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    volumeMounts:
    - name: config-volume
      mountPath: /config
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 3
      periodSeconds: 3
  volumes:
  - name: config-volume
    configMap:
      name: app-config
  restartPolicy: Always
  affinity: # 친화성 설정
    podAntiAffinity: # 파드에 대한 반친화 설정 (어떤 파드랑은 같은 노드에 배포가 안되게끔)
      requiredDuringSchedulingIgnoredDuringExecution: # 새로운 스케줄링에 대해서만 적용
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - myapp
        topologyKey: kubernetes.io/hostname
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
```







