# modu-blog-project
FastAPI + SQLite + SQLAlchemy + JWT + MPA(Multi-Page Application)
단일 FastAPI 서버에서 API + HTML을 모두 제공하는 블로그 프로젝트

📌 주요 기능
🔐 회원 기능

회원가입

로그인 (JWT 발급)

JWT 기반 인증

인증된 사용자만 글 작성/수정/삭제 가능

본인이 작성한 글만 수정/삭제 가능

📝 블로그 기능

게시글 목록 조회

게시글 상세 조회

게시글 작성(로그인 필요)

게시글 수정(로그인 + 본인 글)

게시글 삭제(로그인 + 본인 글)

🎨 MPA 프론트엔드

/static/ 디렉토리에서 HTML 파일 서빙

로그인/회원가입/목록/상세/생성/수정 페이지 제공

fetch API로 FastAPI 서버와 통신

localStorage에 토큰 저장

🗂️ 프로젝트 구조
modu-blog-project/
│
├── main.py
├── requirements.txt
│
├── database/
│   ├── __init__.py
│   ├── connection.py
│   ├── models.py
│
├── core/
│   ├── auth.py              # JWT / 비밀번호 해싱 / 인증 유틸
│
├── routers/
│   ├── auth.py              # 회원가입, 로그인
│   ├── blog.py              # 블로그 CRUD
│
├── static/                  # MPA HTML 파일
│   ├── blog_list.html
│   ├── blog_detail.html
│   ├── blog_create.html
│   ├── blog_edit.html
│   ├── login.html
│   ├── register.html

💡 기술 스택
분야	기술
Backend	FastAPI, SQLAlchemy, SQLite
Auth	JWT (python-jose), bcrypt(passlib)
Frontend	HTML, CSS, JavaScript (MPA), Fetch API
Data	Pydantic 모델
ETC	CORS, StaticFiles
🚀 설치 및 실행
1) 저장소 클론
git clone https://github.com/사용자명/modu-blog-project.git
cd modu-blog-project

2) 가상환경 생성 및 활성화 (선택)
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows

3) 의존성 설치
pip install -r requirements.txt


또는 직접 설치

pip install fastapi uvicorn sqlalchemy pydantic python-jose[cryptography] passlib[bcrypt] python-multipart

4) FastAPI 서버 실행
uvicorn main:app --reload

5) 서비스 접속

브라우저에서 아래 주소로 이동:

📄 로그인 페이지

http://127.0.0.1:8000/static/login.html


📝 블로그 목록 페이지

http://127.0.0.1:8000/static/blog_list.html

🔑 인증 흐름 (JWT)

클라이언트가 /token 엔드포인트로 로그인 요청

비밀번호 검증 성공 시 JWT 발급

클라이언트는 localStorage에 토큰 저장

보호된 API 호출 시 헤더에 토큰 포함

Authorization: Bearer <token>


FastAPI는 get_current_user() 로 토큰 검증

유효한 토큰이면 현재 사용자 정보를 반환

📡 API 문서
📌 Auth API
🧪 회원가입
POST /register


Body(JSON)

{
  "email": "example@test.com",
  "password": "1234"
}

🔐 로그인(JWT 발급)
POST /token


Body(form-data)

username (email)

password

응답

{
  "access_token": "...",
  "token_type": "bearer"
}

📌 Blog API
게시글 목록 조회
GET /blogs

게시글 상세 조회
GET /blogs/{id}

게시글 생성 (인증 필요)
POST /blogs

게시글 수정 (본인 글 + 인증 필요)
PUT /blogs/{id}

게시글 삭제 (본인 글 + 인증 필요)
DELETE /blogs/{id}

🎨 프론트엔드 사용 예시
토큰 가져오기
const token = localStorage.getItem('token');

인증 API 호출 예시
fetch("http://localhost:8000/blogs", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${token}`
    },
    body: JSON.stringify({ title, content })
});
