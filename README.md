# Docker-in-ubuntu

## 🚦목차
<a href="#개요">1. 개요</a><br>
<a href="#프로젝트-설명">2. 프로젝트 설명</a><br>
<a href="#팀원">3. 팀원</a><br>
<a href="#활용-기술">4. 활용 기술</a><br>
<a href="#구현">5. 구현</a><br>
<a href="#트러블-슈팅">6. 트러블 슈팅</a><br>
<a href="#회고">7. 회고</a><br>

## 🖥️**개요**
 
## 📒**프로젝트 설명**

## 👊**팀원**
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

## 🔍**활용 기술**
<table style="border-collapse: collapse;">
<tr>
    <td><img src="https://techstack-generator.vercel.app/docker-icon.svg" alt="Docker" width="65" height="65" /></td>
</tr>
</table>

## **구현**
### Dockerfile
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
### 애플리케이션 파일 준비
Spring 애플리케이션을 Docker로 배포하기 위해 필요한 파일들을 준비합니다.
```bash
ubuntu@myserver01:~/springImg$ ls
Dockerfile  springapp.jar
```
### Docker 이미지 빌드
Spring 애플리케이션을 포함한 Docker 이미지를 생성합니다.
```bash
ubuntu@myserver01:~/springImg$ docker build -t springapp .
```
### 생성된 Docker 이미지 확인
```bash
ubuntu@myserver01:~/springImg$ docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
springapp             latest    a1a78cf3c43f   2 minutes ago    205MB
```
### 태그 변경 후 Docker Hub에 푸시 준비
```bash
ubuntu@myserver01:~/springImg$ docker tag springapp:latest danidanacdy/springapp:1.0
```
### 태그 변경 확인
```bash
ubuntu@myserver01:~/springImg$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   7 minutes ago    205MB
springapp               latest    a1a78cf3c43f   7 minutes ago    205MB
```
### Docker Hub에 푸시
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

### 다른 서버에서 Docker Hub에 올린 이미지 가져오기
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
### 다운로드된 이미지 확인
```bash
ubuntu@myserver01:~$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   27 minutes ago   205MB
```
### 컨테이너 실행
```bash
ubuntu@myserver01:~$ docker run -d -p 8081:8081 --name daniSpringApp danidanacdy/springapp:1.0
66394cecd5bcbb4503cf475d2c37e2f43ac91481aed4c6ee1c61c7a3998efc4e
```
### 실행 중인 컨테이너 확인
```bash
ubuntu@myserver01:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS                                         NAMES
66394cecd5bc   danidanacdy/springapp:1.0   "java -jar springapp…"   6 minutes ago   Up 6 minutes   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp   daniSpringApp
```

### 다른 사용자 Docker Hub에서 이미지 가져오기
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
### 다운로드된 이미지 확인
```bash
ubuntu@myserver01:~$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
danidanacdy/springapp   1.0       a1a78cf3c43f   38 minutes ago   205MB
woody6624/springapp     1.0       70351ec7fa08   2 hours ago      205MB
```
### 컨테이너 실행 (다른 포트 사용)
```bash
ubuntu@myserver01:~$ docker run -d -p 8082:8079 --name woodySpringApp woody6624/springapp:1.0
7faa8fdfccd3adbe00fb043aa79672e3359b25f4bbaf0f755bdca7f5ed8909b5
```
### 실행 중인 컨테이너 확인
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
## **회고**
