## Namespace

> 네임스페이스(namespace)는 클러스터 내에서 리소스를 분리하고 관리하는 데 사용되는 방법이다.
> <br>쿠버네티스는 기본적으로 멀티-테넌트 클라우드 네이티브 환경을 지원하기 위해 설계되었으며, 네임스페이스는 이러한 멀티-테넌트 환경을 가능하게 하는 핵심 기능 중 하나이다.

### 네임스페이스의 역할과 중요성
- **리소스 분리**
<br>네임스페이스를 사용하면 동일한 물리적 클러스터를 여러 사용자나 팀, 프로젝트 간에 나누어 사용할 수 있다. 각 네임스페이스는 마치 별도의 클러스터처럼 동작하며, 각기 다른 네임스페이스에 있는 리소스는 서로 (논리적으로)격리되어 있다.
 

- **관리와 조직화**
<br>네임스페이스를 통해 리소스(예 : 파드, 서비스, 볼륨 등)를 논리적으로 그룹화하고 관리할 수 있다. 이는 대규모 시스템에서 효율적인 관리와 운영을 가능하게 한다.

  
- **액세스 제어**
<br>네임스페이스는 액세스 제어와 관련된 규칙을 설정하는 데에도 사용된다. 네임스페이스별로 사용자나 그룹에 대한 권한을 다르게 설정할 수 있어, 보안과 멀티-테넌트 환경을 위한 중요한 도구가 된다.


- **리소스 할당**
<br>네임스페이스를 사용하면 특정 네임스페이스에 리소스(예: CPU, 메모리) 할당량을 지정할 수 있다. 이를 통해 리소스 사용을 효율적으로 관리하고, 공정한 리소스 분배를 보장할 수 있다.


### 네임스페이스 사용 예시
- **개발, 스테이징, 프로덕션 환경 분리**
<br>같은 클러스터 내에서 개발, 테스트, 프로덕션 환경을 별도의 네임스페이스로 구분하여 운영할 수 있다. 이렇게 하면 환경 간 영향을 최소화하면서 리소스를 효율적으로 관리할 수 있다.


- **팀별 리소스 관리**
<br>대규모 조직에서는 각 팀별로 별도의 네임스페이스를 할당하여 리소스를 관리할 수 있다. 이를 통해 팀별로 독립적인 환경을 제공하고, 필요에 따라 리소스를 할당하거나 조절할 수 있다.


- **개인 리소스 관리**
<br>소규모가 사용하는 클러스터 환경에서는 개인을 기준으로 네임스페이스를 생성하여 리소스를 분리하고 관리할 수 있다. 

### 생성

- Yaml
    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      name: my-namespace
      labels:
        name: my-namespace
      annotations:
        description: "my-namespace"
    ```

- CMD
  ```
    kubectl create namespace my-namespace
  ```
  
---

## 멀티태넌시

> 멀티-테넌시는 하나의 소프트웨어 인스턴스나 시스템이 여러 사용자(테넌트)에 의해 공유되어 사용될 수 있도록 하는 아키텍처이다. 이 개념은 주로 클라우드 컴퓨팅과 관련된 서비스에서 흔히 볼 수 있으며, 각 테넌트는 마치 자신만의 독립적인 인스턴스를 사용하는 것처럼 느낄 수 있도록 설계된다. 멀티-테넌시는 효율적인 리소스 공유와 비용 절감을 가능하게 한다.

멀티-테넌시의 핵심은 다음과 같다.

- **리소스 공유**
<br>하나의 애플리케이션, 하드웨어, 또는 소프트웨어 인스턴스가 여러 테넌트에 의해 공유된다.


- **격리와 보안**
<br>각 테넌트의 데이터와 작업은 다른 테넌트로부터 안전하게 격리되어야 한다.


- **사용자 정의와 개인화**
<br>각 테넌트는 필요에 따라 일부 설정을 사용자 정의할 수 있어야 한다.


- **스케일링과 유연성**
<br>시스템은 다양한 테넌트의 요구사항을 수용할 수 있도록 유연하게 확장 가능해야 한다. 
