# S3Demo 프로젝트

이 프로젝트는 AWS S3와 Spring Boot를 활용하여 파일 업로드, 이미지 업로드, 파일 삭제 기능을 구현한 예제입니다.  
Postman을 사용하여 API를 테스트하고 연습할 수 있도록 구성되어 있습니다.

---

## **프로젝트 구조**

```plaintext
src/main/java/com/intheeast/
├── S3demoApplication.java      # Spring Boot 애플리케이션의 진입점
├── configuration/
│   └── S3Config.java           # AWS S3 클라이언트 설정
├── controller/
│   └── S3Controller.java       # S3와 상호작용하는 REST API 제공
├── exception/
│   └── S3Exception.java        # S3 관련 사용자 정의 예외 처리
├── service/
│   ├── S3Message.java          # 에러 메시지 관리
│   └── S3Service.java          # S3와 실제로 상호작용하는 비즈니스 로직
└── resources/
    └── application.yml         # 프로젝트 설정 파일 (AWS 인증 정보 및 버킷 정보 포함)
```

---

## **Postman을 사용한 API 테스트**

### 1. **파일 업로드**
- **URL**: `/s3/upload`
- **Method**: `POST`
- **Headers**:
  - `Content-Type: multipart/form-data`
- **Body** (Form-Data):
  - Key: `file`
  - Value: 업로드할 파일
- **Response**:
  - 성공 시: 업로드된 파일의 S3 URL 반환.
  - 실패 시: 에러 메시지.

---

### 2. **이미지 업로드**
- **URL**: `/s3/upload-image`
- **Method**: `POST`
- **Headers**:
  - `Content-Type: multipart/form-data`
- **Body** (Form-Data):
  - Key: `file`
  - Value: 업로드할 이미지 파일 (`.jpg`, `.png` 등)
- **Response**:
  - 성공 시: 업로드된 이미지의 S3 URL 반환.
  - 실패 시: 에러 메시지.

---

### 3. **파일 삭제**
- **URL**: `/s3/delete/{fileName}`
- **Method**: `DELETE`
- **Path Variable**:
  - `fileName`: 삭제할 파일 이름
- **Response**:
  - 성공 시: `"파일 삭제 성공"`
  - 실패 시: 에러 메시지.

---

## **API 테스트 시 주의 사항**

1. **Postman 설정**
   - 올바른 HTTP Method(`POST`, `DELETE`)와 URL(`/s3/...`)을 확인하세요.
   - 업로드할 파일은 Form-Data 형식으로 Body에 추가하세요.

2. **AWS 권한 설정**
   - 버킷에서 업로드 및 삭제 권한이 제대로 부여되었는지 확인하세요.

3. **CORS 설정**
   - S3 버킷에서 아래와 같이 CORS를 설정:
     ```json
     [
       {
         "AllowedHeaders": ["*"],
         "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
         "AllowedOrigins": ["*"],
         "ExposeHeaders": ["ETag"]
       }
     ]
     ```

---

## **유효성 검증 로직**

### 1. 파일 유효성 검증 (`isValidFile`)
- 허용되지 않는 확장자: `.exe`, `.bat`
- 잘못된 파일 업로드 시 **`INVALID_FILE`** 예외 발생.

### 2. 이미지 유효성 검증 (`isValidImage`)
- 허용되는 확장자: `.jpg`, `.png`
- 잘못된 이미지 업로드 시 **`INVALID_IMAGE`** 예외 발생.

---

## **예외 처리**

`S3Exception`을 통해 다음과 같은 사용자 정의 메시지를 반환합니다:
- **`INVALID_FILE`**: 유효하지 않은 파일 형식.
- **`INVALID_IMAGE`**: 유효하지 않은 이미지 형식.

---

## **기술 스택**

- **Backend**: Spring Boot, AWS SDK
- **Java Version**: 17
- **Build Tool**: Maven
- **Cloud**: AWS S3

---

## **문제 해결**

### 1. **Postman 요청 실패**
- **원인**: 잘못된 URL 또는 HTTP Method 사용.
- **해결**: API URL과 요청 형식을 다시 확인.

### 2. **권한 문제**
- **원인**: S3 버킷에 적절한 읽기/쓰기/삭제 권한이 설정되지 않음.
- **해결**: AWS IAM 정책 및 S3 버킷 정책 수정.

### 3. **파일 업로드 실패**
- **원인**: 잘못된 파일 형식.
- **해결**: 업로드할 파일의 확장자를 확인하고 지원되는 형식인지 확인.

---

