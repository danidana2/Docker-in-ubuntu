# Docker Image Slimming: 더 작고 빠른 컨테이너 만들기

## 🚦 목차
<a href="#개요">1. 개요</a><br>
<a href="#프로젝트-설명">2. 프로젝트 설명</a><br>
<a href="#팀원">3. 팀원</a><br>
<a href="#활용-기술">4. 활용 기술</a><br>
<a href="#구현">5. 구현</a><br>
<a href="#트러블-슈팅">6. 트러블 슈팅</a><br>

## 🖥️ **개요**
이 프로젝트는 Spring Boot 애플리케이션(JAR 파일)을 활용하여, **최적화**된 Dockerfile을 통해 Docker 이미지를 생성하고 Docker Hub에 업로드하는 과정을 다룹니다.
Dockerfile에 적용한 **최적화** 기법(경량 JRE 기반 이미지 사용)을 통해 이미지 크기를 최소화하고 빌드 시간을 단축하여 배포 및 실행 효율성을 극대화하며, 이를 다양한 환경에서 손쉽게 다운로드하여 사용할 수 있도록 합니다.

## 📒 **프로젝트 설명**
이미 빌드된 Spring Boot 애플리케이션(JAR 파일)을 기반으로, 최적화된 Dockerfile을 사용하여 Docker 이미지를 빌드하는 과정을 중심으로 진행됩니다.
이 과정에서 경량 JRE 기반 Alpine 이미지를 채택하는 최적화 기법을 적용함으로써, **이미지 크기를 최소화**하고 **빌드 시간을 단축**하는 동시에, Docker Hub에 업로드하여 다양한 환경에서 손쉽게 다운로드 및 배포할 수 있도록 구성되어 있습니다.

### 프로젝트 주요 기능
- **JAR 파일을 Docker 이미지로 전환** <br> 
기존에 빌드된 Spring Boot JAR 파일을 활용하여, 애플리케이션을 Docker 컨테이너 환경에서 실행할 수 있도록 이미지로 변환합니다.
- **이미지 최적화** <br> 
최적화된 Dockerfile을 통해 경량 JRE 기반 Alpine 이미지를 사용하고, 불필요한 파일을 제거하는 등의 기법을 적용하여 Docker 이미지의 크기를 최소화합니다.
- **Docker Hub 업로드 및 배포** <br> 
최적화된 Docker 이미지를 Docker Hub에 업로드하여, 다른 환경에서도 손쉽게 이미지 pull 및 컨테이너 실행이 가능하도록 지원합니다.
- **다운로드 후 실행** <br> 
Docker Hub에 등록된 이미지를 활용하여, 간단한 명령어로 컨테이너를 실행하고 애플리케이션을 배포할 수 있습니다.

## 🤝 **팀원**
<table>
  <tbody>
    <tr>
      <td align="center">
         <a href="https://github.com/danidana2">
          <img src="https://avatars.githubusercontent.com/u/150885774?v=4" width="150px;" alt=""/>
          <br /><sub><b> 최다영 </b></sub>
        </a>
        <br />
      </td>
      <td align="center">
          <a href="https://github.com/woody6624">
          <img src="https://avatars.githubusercontent.com/u/103871252?v=4" width="150px;" alt=""/>
          <br /><sub><b> 김우현 </b></sub>
        </a>
        <br />
      </td>
  </tbody>
</table>

## 🛠 **활용 기술**
| 분류      | 기술   | 
|:-      |:-     |
| **Container**      | ![Docker Badge](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white&style=for-the-badge)      |
| **Registry**       | ![Docker Hub Badge](https://img.shields.io/badge/Docker_Hub-2496ED?logo=docker&logoColor=white&style=for-the-badge)    | 
| **OS**             | ![Ubuntu Badge](https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&logoColor=white&style=for-the-badge)    | 
| **Communication**  | ![Slack Badge](https://img.shields.io/badge/Slack-4A154B?logo=slack&logoColor=white&style=for-the-badge)     | 
| **Virtualization** | ![VirtualBox Badge](https://img.shields.io/badge/VirtualBox-183A61?logo=virtualbox&logoColor=white&style=for-the-badge) ![VMware Badge](https://img.shields.io/badge/VMware-607078?logo=vmware&logoColor=white&style=for-the-badge) | 

## ⚙️ **구현**

### 1. 최적화된 Dockerfile 구성 및 선택 배경
```bash
# 1. 가벼운 JRE 기반 이미지 사용 (최소한의 실행 환경)
FROM eclipse-temurin:17-jre-alpine

# 2. 작업 디렉토리 설정
WORKDIR /app

#3. 로컬에서 빌드된 JAR 파일을 컨테이너로 복사
COPY ./springapp.jar springapp.jar

#4. 포트번호
EXPOSE 8081

#4. 작동
ENTRYPOINT ["java", "-jar", "springapp.jar"]
```
- **JRE 선택** <br>
Docker 이미지 크기를 최소화하기 위해, Java Development Kit (JDK) 대신 Java Runtime Environment (JRE)를 사용했습니다. <br>
JRE는 실행에 필요한 최소한의 라이브러리만 포함하므로, 불필요한 개발 도구들이 제거되어 이미지가 훨씬 가볍습니다. <br>

- **eclipse-temurin:17-jre-alpine 버전 선택** <br>
Alpine Linux 기반 이미지는 기본적으로 매우 경량화되어 있어, 이미지 빌드 및 배포 시 효율적입니다. <br>
17 버전은 최신 LTS(Long Term Support) 버전으로 안정성과 장기 지원 측면에서 우수하며, 기존 애플리케이션과의 호환성이 보장됩니다. <br> <br>

**최적의 베이스 이미지 선택을 통해 경량화 및 성능 향상을 도모하였습니다.** <br>
**추후 multi-stage build 등의 기법을 추가하면, 빌드 시 불필요한 파일을 더욱 효과적으로 제거할 수 있습니다.**
<br>

> **이미지 최적화의 이점** <br>
파일 크기 최소화로 인해 네트워크 전송 및 저장 공간 사용량이 줄어듭니다.
빌드 시간이 단축되며, 배포 시 컨테이너 실행이 신속해집니다.
경량화된 이미지는 보안 측면에서도 유리하며, 불필요한 구성요소가 제거되어 공격 표면이 줄어듭니다.

### 2. 애플리케이션 파일 준비
Spring 애플리케이션을 Docker로 배포하기 위해 필요한 파일들을 준비합니다.
```bash
ubuntu@myserver01:~/springImg$ ls
Dockerfile  springapp.jar
```
### 3. Docker 이미지 빌드
Spring 애플리케이션을 포함한 Docker 이미지를 생성합니다.
```bash
ubuntu@myserver01:~/springImg$ docker build -t springapp .
```
### 4. 생성된 Docker 이미지 확인
```bash
ubuntu@myserver01:~/springImg$ docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
springapp             latest    a1a78cf3c43f   2 minutes ago    205MB
```
### 5. 태그 변경 후 Docker Hub에 푸시 준비
```bash
ubuntu@myserver01:~/springImg$ docker tag springapp:latest danidanacdy/springapp:1.0
```
### 6. 태그 변경 확인
```bash
ubuntu@myserver01:~/springImg$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   7 minutes ago    205MB
springapp               latest    a1a78cf3c43f   7 minutes ago    205MB
```
### 7. Docker Hub에 푸시
```bash
ubuntu@myserver01:~/springImg$ docker push danidanacdy/springapp:1.0
The push refers to repository [docker.io/danidanacdy/springapp]
d5554d6cc319: Pushed
20d0fa9040fa: Pushed
872b4640f1c3: Mounted from library/eclipse-temurin
b1bdb6e103f3: Mounted from library/eclipse-temurin
30e00ad713b7: Mounted from library/eclipse-temurin
1cd01d0f38a4: Mounted from library/eclipse-temurin
08000c18d16d: Mounted from library/eclipse-temurin
1.0: digest: sha256:40cc7f8a084e1d2afa4ae360dcf7e7b48494e581a7e26a323443d8c2f009dac1 size: 1785
```
![alt text](images/dockerhub1.png)
![alt text](images/dockerhub2.png)

### 8. 다른 서버에서 Docker Hub에 올린 이미지 가져오기
```bash
ubuntu@myserver01:~$ docker pull danidanacdy/springapp:1.0
1.0: Pulling from danidanacdy/springapp
f18232174bc9: Pull complete
e6cc66ba0a96: Pull complete
d2830fdce3f8: Pull complete
d359e0dcce06: Pull complete
51631fe5dc90: Pull complete
8bb28b7b32ad: Pull complete
aa154074f5a8: Pull complete
Digest: sha256:40cc7f8a084e1d2afa4ae360dcf7e7b48494e581a7e26a323443d8c2f009dac1
Status: Downloaded newer image for danidanacdy/springapp:1.0
docker.io/danidanacdy/springapp:1.0
```
### 9. 다운로드된 이미지 확인
```bash
ubuntu@myserver01:~$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   27 minutes ago   205MB
```
### 10. 컨테이너 실행
```bash
ubuntu@myserver01:~$ docker run -d -p 8081:8081 --name daniSpringApp danidanacdy/springapp:1.0
66394cecd5bcbb4503cf475d2c37e2f43ac91481aed4c6ee1c61c7a3998efc4e
```
### 11. 실행 중인 컨테이너 확인
```bash
ubuntu@myserver01:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS                                         NAMES
66394cecd5bc   danidanacdy/springapp:1.0   "java -jar springapp…"   6 minutes ago   Up 6 minutes   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp   daniSpringApp
```

### 12. 다른 사용자 Docker Hub에서 이미지 가져오기
```bash
ubuntu@myserver01:~$ docker pull woody6624/springapp:1.0
1.0: Pulling from woody6624/springapp
f18232174bc9: Pull complete
e6cc66ba0a96: Pull complete
d2830fdce3f8: Pull complete
d359e0dcce06: Pull complete
51631fe5dc90: Pull complete
59ac075c6e11: Pull complete
7b63dd6d2533: Pull complete
Digest: sha256:a60c77cc13e3397d1cd9d68061afcbcfb6aa7ab0295a098dc27a367565553b6c
Status: Downloaded newer image for woody6624/springapp:1.0
docker.io/woody6624/springapp:1.0
```
### 13. 다운로드된 이미지 확인
```bash
ubuntu@myserver01:~$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   38 minutes ago   205MB
woody6624/springapp     1.0       70351ec7fa08   2 hours ago      205MB
```
### 14. 컨테이너 실행 (다른 포트 사용)
```bash
ubuntu@myserver01:~$ docker run -d -p 8082:8079 --name woodySpringApp woody6624/springapp:1.0
7faa8fdfccd3adbe00fb043aa79672e3359b25f4bbaf0f755bdca7f5ed8909b5
```
### 15. 실행 중인 컨테이너 확인
```bash
ubuntu@myserver01:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS                                                   NAMES
7faa8fdfccd3   woody6624/springapp:1.0     "java -jar springapp…"   53 seconds ago   Up 52 seconds   8081/tcp, 0.0.0.0:8082->8079/tcp, [::]:8082->8079/tcp   woodySpringApp
66394cecd5bc   danidanacdy/springapp:1.0   "java -jar springapp…"   11 minutes ago   Up 11 minutes   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp             daniSpringApp
```

## 🔫**트러블 슈팅**
### Dockerfile - EXPOSE의 정체

참고 : https://docs.docker.com/reference/dockerfile/

`EXPOSE`: Describe which ports your application is listening on.

`EXPOSE`에 특정 포트를 명시적으로 등록하면, **해당 Dockerfile로 만들어지는 이미지가 그 포트를 컨테이너 내부에서 사용할 것이라고 "문서화"하는 역할을 한다.** 하지만 **실제로 포트가 외부에 노출되거나 접근 가능해지는 것은 아니다.**

---

## 📌 `EXPOSE`를 설정한 경우, 이미지의 특징

**Dockerfile**

```bash
# 1. 가벼운 JRE 기반 이미지 사용 (최소한의 실행 환경)
FROM eclipse-temurin:17-jre-alpine

# 2. 작업 디렉토리 설정
WORKDIR /app

#3. 로컬에서 빌드된 JAR 파일을 컨테이너로 복사
COPY ./springapp.jar springapp.jar

#4. 포트번호
EXPOSE 8081

#4. 작동
ENTRYPOINT ["java", "-jar", "springapp.jar"]
```

해당 Dockerfile을 빌드하면 `8081` 포트가 컨테이너 내부에서 열린다는 정보가 이미지에 포함됩니다.

하지만, **이 이미지를 실행할 때 추가 설정 없이 `-p` 옵션을 사용하지 않으면, 외부에서 8081 포트로 접속할 수 없습니다.**

---

## ✅ `EXPOSE`가 적용된 이미지의 특징

### 1️⃣ p 옵션을 주지 않고 실행

```bash
docker run springapp
```

- 이 경우 **`EXPOSE 8081`만 설정되어 있기 때문에, 외부에서 접근할 수 없다.**
- 컨테이너 내부에서 `8081` 포트는 사용할 수 있지만 호스트 시스템이나 외부 네트워크에서는 접근 불가능합니다.

```bash
PORTS
8081/tcp
```

---

### 2️⃣ `p` 옵션 없이 `P`를 사용하면?

```bash
docker run -p springapp
```

- `EXPOSE`된 포트를 **랜덤한 호스트 포트**에 매핑합니다.
- 해당 예시에서는 `32768` 포트에 매핑되었습니다.
- 브라우저나 API 요청으로 `http://IP주소:32768`에서 접근 가능.

```bash
PORTS
0.0.0.0:32768->8081/tcp, [::]:32768->8081/tcp
```

---

### 3️⃣ `p` 옵션을 사용하면?

```
docker run -p 8081:8081 springapp
```

- 이제 **호스트의 `8081` 포트가 컨테이너의 `8081` 포트와 직접 연결됨.**
- 브라우저나 API 요청으로 `http://IP주소:8081`에서 접근 가능.

```bash
PORTS
0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp
```

### 3️⃣ `p` 옵션을 사용하지만 EXPOSE에 작성한 포트가 아닌 포트로 작성한다면?

```bash
docker run -p 8090:8079 springapp
```

- 브라우저나 API 요청으로 `http://IP주소:8090` 에서 접근 가능
- 컨테이너 내부의 8081 포트는 EXPOSE 때문에 열려 있지만,
호스트에서 연결된 건 8090 -> 8079 포트이므로 8081은 외부에서 접근 불가.

```bash
PORTS
8081/tcp, 0.0.0.0:8090->8079/tcp, [::]:8090->8079/tcp
```

