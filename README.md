👋 Hi, I’m @ktjw1125

😄 프로젝트 목표 : 라즈베리파이를 통해 서비스되는 웹페이지의 index.html 파일수정시 github action 워크플로우를 통해 자동 배포 환경 구축 (수행기간 2024.10.21-22)

⚡ 결과
index.html 파일 : https://github.com/ktjw1125/rpi_webpage_continuous_deploy/blob/main/index.html

워크플로우 yml파일 : https://github.com/ktjw1125/rpi_webpage_continuous_deploy/blob/main/.github/workflows/cicd_webpage.yml

라즈베리파이 웹페이지 : http://211.201.153.117:8000/



🌱 진행 과정

작업환경 : 라즈베리파이(rpi)

(라즈베리파이(rpi) 포트포워딩)
192.168.1.x (라즈베리파이 ip) (port1/port2/port3으로 들어올시 80/8080/22으로 포트포워딩)
192.168.1.1 (개인공유기 게이트웨이)

192.168.55.x (개인공유기 ip) (port1/port2/port3로 들어올시 port1/port2/port3으로 포트포워딩) (외부포트에 22, 80, 82, 161, 162, 8079 ~ 8096 사용불가)
192.168.55.1 (skb모뎀 게이트웨이)
211.201.x.x (공인IP) (skb모뎀 주소)


(apache2 웹서버 설치)
sudo apt update
sudo apt install apache2 -y

(github 환경준비)
ssh 인증 (github과 rpi간 코드관리용)
작업디렉토리 준비 (~/github)
원격 repo 생성 및 clone으로 연결
작업자 신원 입력
cp /var/www/html/index.html . (기존 사용하던 index.html 파일 있다면 복사해오기)

(github action ci/cd 워크플로우 yml파일 작성)
ssh 인증서 준비 (github action 컨테이너와 rpi간 자동배포용)
mkdir -p .github/workflows
vi .github/workflows/cicd_webpage.yml

(git push 및 자동배포 확인)
git add .
git commit -m "comment"
git push

github웹에서 자동배포 내역확인 (실패시 이메일도 온다)
gh 설치하면 CLI환경에서도 워크플로우 확인가능




<< github 기본적인 사용방법 >>

(github 설치)
sudo apt-get install git -y

(github ssh key 기반인증)
ssh-keygen -f ~/.ssh/id_rsa-github-ktjw1125 -C "ktjw1125@gmail.com"
ls -l ~/.ssh
cat ~/.ssh/id_rsa-github-ktjw1125.pub
pub 내용을 github settings > ssh and gpg keys > new ssh key로 등록 (title은 jwrpicli_241021, key type은 authentication key, key내용 복붙)

vi ~/.ssh/config
Host github.com
User fit
Hostname github.com
IdentityFile ~/.ssh/id_rsa-github-ktjw1125

ssh -T git@github.com (이제 토큰,암호 필요없음)

(github 웹에서 repo 생성)
(clone) - 로컬 생성하며 pull
mkdir -p ~/github
cd ~/github
git clone git@github.com:ktjw1125/rpi_webpage_continuous_deploy.git   (clone하면 디렉토리 생기고 복제되면서 연결됨) (바로push가능)

(git 사용자정보 등록)
git config --local user.name "jwlee_local"
git config --local user.email "ktjw1125@gmail.com"
cat ./.git/config

(git branch 관리)
git branch (확인)
git branch -m master main (이름변경)
git switch 브랜치 전환
git restore 마지막 커밋 상태로 파일 복구

(repo 수동 추가/변경)
git init (빈 디렉토리를 git관리 시작)
git remote add origin https://github.com/ktjw1125/rpi_webpage (암호 요구하므로 사용불가)
git remote add origin git@github.com:ktjw1125/rpi_webpage.git (ssh설정 후 사용가능)
git remote remove origin
git remote set-url origin git@github.com:ktjw1125/rpi_webpage.git (repo주소 변경시)
git remote -v (확인)

(pull) - repo 최신화
git fetch origin (가져오기, merge제외)
git merge origin/main (로컬 브랜치와 원격 브랜치의 변경 사항 병합)
git config pull.rebase true (divergent branches 커밋내역 충돌날떄)
git pull origin main (fetch + merge)

(push)
vi index.html
git diff HEAD^..HEAD (스테이징되지 않은 변경사항 확인) (세부코드까지 확인)
git diff --staged (스테이징된 변경사항 확인)
git status (변경사항 확인)
git add .  (스테이징)
git commit -m "comment "(-a옵션 사용시 새로운파일제외 staging + commit)
git log origin/main (원격브랜치 커밋 히스토리 확인)
git push
git rebase origin/main (공유repo 브랜치 직선으로 정리)
git push --set-upstream origin main (pull로 병합되어 repo와 연결 안된 경우)
git push origin main:main (로컬:원격)

(커밋 수정)
git reset --hard HEAD~1 (변경 내용도 삭제)
git reset --soft HEAD~1  (파일 수정 내용은 유지)
git commit --amend (메세지 수정)

(파일삭제)
git rm 파일명 (로컬에서 파일 삭제 + Git에서 추적해제 + 스테이징 반영)
git status
git commit -m "삭제한 파일에 대한 설명"
git push origin main
