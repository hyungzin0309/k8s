# Secret

> 쿠버네티스(Kubernetes)에서 'Secret'은 중요한 정보를 보관하기 위한 리소스이다.
> <br>Secret을 사용하면 비밀번호, OAuth 토큰, ssh 키와 같은 민감한 데이터를 저장하고 관리할 수 있다. 이러한 데이터는 애플리케이션 코드나 컨테이너 이미지에 직접 포함시키는 것보다는 Secret을 통해 별도로 관리하는 것이 안전하다.

## 사용 상황

  -  **인증 정보**: 데이터베이스 비밀번호, API 키, 서비스 계정 토큰 등 인증에 필요한 정보를 보관한다.
  -  **TLS 인증서**: HTTPS 통신을 위한 TLS 인증서 및 관련 키를 저장한다.
  -  **Config 파일**: 애플리케이션 설정 파일 같은 민감한 설정 정보를 담는다.

## 활용 예시

### 1. secret 생성
- Yaml
    ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: my-secret
    type: Opaque
    data:
      password: [Base64 인코딩된 비밀번호]
    ```
- kubectl 명령어 1

    ```script
    kubectl create secret generic my-secret --from-literal=username=myusername --from-literal=password=mypassword
    ```
  * 명령어로 생성 시, value 를 평문으로 입력해도 자동으로 base64 인코딩된다!


- kubectl 명령어 2 (key : 파일명 / value : 파일 내용)

    ```script
    kubectl create secret generic my-secret --from-file=path/to/myfile
    ```


### 2-1. Pod 에 적용 (ENV)

- Yaml

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
      - name: mycontainer
        image: myimage
        env:
          - name: PASSWORD
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: password
    ```
  
### 2-2. Pod 에 적용 (Volume)
- Yaml
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
      - name: mycontainer
        image: myimage
        volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secret"
          readOnly: true
      volumes:
      - name: secret-volume
        secret:
          secretName: my-secret
    ```

