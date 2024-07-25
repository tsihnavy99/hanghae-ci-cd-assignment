## 프론트엔드 CI/CD 파이프라인

### 개요

GitHub Actions에 워크플로우를 작성해 다음과 같이 배포가 진행되도록 함

1. 저장소를 체크아웃합니다.
2. Node.js 18.x 버전을 설정합니다.
3. 프로젝트 의존성을 설치합니다.
4. Next.js 프로젝트를 빌드합니다.
5. AWS 자격 증명을 구성합니다.
6. 빌드된 파일을 S3 버킷에 동기화합니다.
7. CloudFront 캐시를 무효화합니다.

<br>

### 주요 링크

- S3 버킷 웹사이트 엔드포인트: http://hanghae-plus-tsihnavy.s3-website-us-east-1.amazonaws.com
- CloudFrount 배포 도메인 이름: https://d1pd2vqqn10xxr.cloudfront.net

<br>

### 주요 개념

#### > GitHub Actions과 CI/CD 도구
소프트웨어 개발 workflow 자동화
CI(지속적 통합), CD(지속적 배포) 프로세스 자동화를 통해 개발 주기 단축, 코드 품질 개선 등의 효과를 얻을 수 있음
Jenkins, Bitbucket, Jira Software, Bamboo 등

<br>

#### > S3와 스토리지
S3: '버킷'이라는 리소스에 데이터를 저장
버킷: S3에 저장된 객체에 대한 컨테이너
S3 스토리지 클래스에서 데이터의 이동/저장/액세스 제어/보호/분석 등의 기능 제공

<br>

#### > CloudFront와 CDN
CloudFront: AWS에서 제공하는 CDN 서비스
CDN: 사용자의 위치와 관계없이 컨텐츠를 더욱 빠르게 제공하는 시스템
S3에 파일 업로드 후, CloudFront에 배포 업데이트
CloudFront 배포 ID를 사용해 캐시 무효화 수행

<br>

#### > 캐시 무효화(Cache Invalidation)
기본적으로 CloudFront에서는 24시간동안 오리진의 응답을 캐시해 엣지 로케이션 요청에 캐시된 응답 사용
캐시된 파일의 TTL이 남아있는데 오리진 파일이 변경되는 경우 업데이트가 정상적으로 동작하지 않음
전체 파일, 특정 경로, 특정 파일 포맷을 지정해 캐시 무효화 처리 가능
캐시 무효화 후 접속 시 캐시가 아닌 오리진의 파일을 불러옴

<br>

#### > Repository secret과 환경변수
Repository secret: repository 환경에서 만든 변수, GitHub Actions workflow에서 사용 가능(secret.xx)
환경변수: workflow 파일에서 env 키를 이용해 정의 및 액세스해서 사용