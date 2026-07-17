# 🍊 Ounce 
> **"당신의 아침을 디자인합니다."** 매일 밤 10시, 한정된 수량의 제철 식재료를 특별한 가격에 만나는 미드나이트 타임딜 & 큐레이션 서비스

---

## 🚀 1. 프로젝트 개요
* **서비스명**: Ounce 
* **개발 목적**: 대규모 타임딜 트래픽 상황에서 서버 안정성을 확보하고, 사용자 맞춤형 제철 식재료를 제안하는 MVP(Minimum Viable Product)를 빠르게 구축 및 검증하는 것을 목적으로 합니다.
* **배포 주소**: [https://ouncefresh.com](https://ouncefresh.com)

---

## 🛠️ 2. Tech Stack & Architecture

### Backend & Frontend
* **Core**: Java 17, Spring Boot, Spring Data JPA
* **Security**: Spring Security (Session/Cookie 기반 인증)
* **Database**: MySQL (AWS RDS)
* **Caching & Concurrency**: Redis
* **Infrastructure**: AWS EC2, GitHub Actions (CI/CD)

### System Architecture
사용자 요청부터 무중단 배포 및 백엔드 데이터베이스 영역까지의 전체 구조입니다.

---

## ✨ 3. 핵심 기능 (Key Features)

1. **미드나이트 타임딜 (매일 22:00 ~ 23:00)**
   * 밤 10시에만 열리는 타임딜 한정 수량 식재료 판매 시스템.
   * 실시간 타이머 및 실시간 접속자 수/구매 현황(Ticking) 모킹 처리를 통한 사용자 몰입도 극대화.

2. **사용자 맞춤형 큐레이션**
   * 제철 식재료 및 라이프스타일 기반의 개인화 큐레이션 화면 제공.

3. **Spring Security 기반 멤버십**
   * 안전한 회원가입/로그인 처리 및 로그인 유무에 따른 동적 내비게이션(Navbar) 및 페이지 개인화 렌더링.

---

## 💡 4. 핵심 기술적 고민 (Technical Challenges & Troubleshooting)

### 1️⃣ 동시성 제어 및 트래픽 방어 아키텍처 설계 (백엔드 고도화 진행 중)
* **문제**: 밤 10시 타임딜 오픈 시 특정 인기 식재료에 대규모 동시성 요청이 몰려 재고 정합성이 깨지거나 DB 병목이 발생할 가능성 인지.
* **해결 및 설계**: 
  * DB 비관적 락은 락 대기 시간만큼 커넥션을 점유해 순간 트래픽에서 커넥션 풀 고갈로 이어진다고 판단해, DB 밖에서 원자적으로 차감하는 Redis Lua 스크립트를 단일 게이트로 선택했습니다. 게이트 통과 후 DB 트랜잭션이 실패하면 Redis 재고를 복원하는 보상 로직을 두어 두 저장소 간 정합성이 깨지지 않도록 했습니다.


---

## 📂 5. 시작하기 (How to Run)

### Requirements
* Java 21 (JDK)
* MySQL 8.0+

```bash
# 1. 저장소 클론
git clone [https://github.com/hjStack/Ounce.git](https://github.com/hjStack/Ounce.git)

# 2. 프로젝트 폴더로 이동
cd Ounce

# 3. 빌드 및 실행 (application.yml 설정 확인 필수)
chmod +x mvnw
./mvnw spring-boot:run
