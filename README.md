👋 Hi, I’m [@ktjw1125](https://github.com/ktjw1125)  
<br>

😄 프로젝트 목표 :   

라즈베리파이를 통해 서비스되는 웹페이지의 html 파일 수정시 github action을 통해 자동 배포 <br>
(수행기간 2024.10.21-22)  
<br>

⚡ 결과
- index.html 파일 : <br>
https://github.com/ktjw1125/rpi_webpage_continuous_deploy/blob/main/index.html
- 워크플로우 yml파일 : <br>
https://github.com/ktjw1125/rpi_webpage_continuous_deploy/blob/main/.github/workflows/cicd_webpage.yml
- 라즈베리파이 웹페이지 : <br>
http://211.201.153.117:8000/  
<br>

🌱 진행 과정

작업환경 : 라즈베리파이(rpi)

1. 라즈베리파이(rpi) 포트포워딩<br>
192.168.1.x (라즈베리파이 ip) (port1/port2/port3으로 들어올시 80/8080/22으로 포트포워딩)<br>
192.168.1.1 (개인공유기 게이트웨이)<br><br>
192.168.55.x (개인공유기 ip) (port1/port2/port3로 들어올시 port1/port2/port3으로 포트포워딩) (외부포트에 22, 80, 82, 161, 162, 8079 ~ 8096 사용불가)<br>
192.168.55.1 (skb모뎀 게이트웨이)<br>
211.201.x.x (공인IP) (skb모뎀 주소)<br><br>


2. apache2 웹서버 설치<br>
sudo apt update<br>
sudo apt install apache2 -y<br><br>

3. git 작업환경 준비<br>
ssh 인증 (github과 rpi간 코드관리용)<br>
작업디렉토리 준비 (~/github)<br>
원격 repo 생성 및 clone으로 연결<br>
작업자 신원 입력<br>
cp /var/www/html/index.html . (기존 사용하던 index.html 파일 있다면 복사해오기)<br><br>

4. github action ci/cd 워크플로우 yml파일 작성<br>
ssh 인증서 준비 (github action 컨테이너와 rpi간 자동배포용)<br>
github secret변수에 개인키, ip, 포트 등 민감정보 등록 <br>
mkdir -p .github/workflows<br>
vi .github/workflows/cicd_webpage.yml<br><br>

5. git push 및 자동배포 확인<br>
git add .<br>
git commit -m "comment"<br>
git push<br><br>

github웹에서 자동배포 내역확인 (실패시 이메일 발송)<br>
gh 설치하면 CLI환경에서도 워크플로우 확인가능<br>

<img width="971" alt="image" src="https://github.com/user-attachments/assets/87cf42a5-06fb-4100-ba07-203d72f27454">

