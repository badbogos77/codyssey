1. 실행환경
   OS : WINDOW
   SHELL : WSL2 Ubuntu / bash
   DOCKER : docker --version
   GIT : git --version

2. 환경설치
   wsl : https://learn.microsoft.com/ko-kr/windows/wsl/install
   git : https://git-scm.com/download/win
   docker desktop : https://www.docker.com/products/docker-desktop/
   github 가입 : https://github.com/

2-1. wsl 설치
(powershell)
wsl --install -d Ubuntu-22.04
wsl --status
설치 후 재부팅 -> 최초 실행시 사용자이름/비밀번호 설정

2-2. git 설치 후 (powershell) git --version으로 확인

2-3. docker desktop 설치
(docker desktop 실행 후 오르쪽 상단에 톱니바퀴 아이콘 눌러서 셋팅 수정)

- Settings → General → Use the WSL 2 based engine 체크
- Settings → Resources → WSL Integration → Ubuntu 활성화

3. WSL 조작방법
   (powershell)
   wsl 치고 엔터치면 wsl 창으로 이동됨

---

pwd # 현재 위치
ls -la # 숨김 파일 포함 목록
mkdir -p ~/codyssey/practice # 생성
cd ~/codyssey/practice # 이동
touch empty.txt # 빈 파일 생성
echo "hello" > note.txt # 내용이 있는 파일
cat note.txt # 내용 확인
cp note.txt note_copy.txt # 복사
mv note_copy.txt renamed.txt # 이름 변경
mkdir backup && mv renamed.txt backup/ # 이동
ls -la . backup
rm empty.txt # 삭제
rm -r backup # 디렉토리 삭제

절대경로: 최상위 폴더(/)부터 시작
상대경로: 현재위치 기준으로 시작 예) .. (상위폴더)

---

권한실습
touch perm_test.txt && mkdir perm_dir

# 변경 전

ls -l perm_test.txt ; ls -ld perm_dir

# 파일: 644 -> 600 -> 755

chmod 600 perm_test.txt ; ls -l perm_test.txt
chmod 755 perm_test.txt ; ls -l perm_test.txt

d rwx rwx rwx
│ │ │ └─ 기타(others)
│ │ └─ 그룹(group)
│ └─ 소유자(owner)
└─ 파일 종류 (d = 디렉토리, - = 파일)

r 읽기, w 수정, x 실행
r=4
w=2
x=1
예) 777 (rwxrwxrwx)
700 (rwx------)
644 (rw-r--r--)

---

![버전 확인](images/image.png)

![리눅스 조작방법](images/image-1.png)

![파일 권한변경](images/image-2.png)

4. Dockerfile 생성 및 site/index.html 생성

4-1.폴더 구조
codyssey
├── README.md
├── site/  
├── Dockerfile
├── images/

4-2.site/index.html파일 작성

<!doctype html>
<html lang="ko"><body><h1>Hello from my custom nginx</h1></body></html>

4-3. Dockerfile 파일 작성
FROM nginx:alpine
COPY site/ /usr/share/nginx/html/

4-4. bash에서 Dockerfile로 이미지 생성 및 실행방법
docker build -t my-web:1.0 .
docker images my-web
docker run -d -p 8080:80 --name my-web-8080 my-web:1.0
docker logs my-web-8080
curl http://localhost:8080

![Dockerfile build](images/image-3.png)

![서버 실행 중](images/image-4.png)

5. git 설정 및 github 연동
   (bash)
   git config --global user.name "내 이름"
   git config --global user.email "내 GitHub 이메일"
   git config --global init.defaultBranch main
   git config --list

![git config 설정](images/image_config.png)

6. vscode 터미널에서
   cd ~/codyssey
   git init
   git add .
   git status # 민감정보 포함 여부 마지막 확인
   git commit -m "docs: add workstation setup README and Dockerfile"
   git branch -M main
   git remote add origin https://github.com/<내-아이디>/codyssey-workstation.git
   git remote -v
   git push -u origin main
