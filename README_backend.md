# Endpoints

## Base URL

http://localhost:5000/


## 건물 및 교실 정보 API

### `GET /buildings`

- **설명**: 등록된 모든 건물 정보를 반환합니다.
  특정 건물 또는 교실 조회는 `?id=건물ID` 또는 `?id=건물ID-교실ID` 형식을 사용합니다.

#### **예시 요청**

```http
GET /buildings?id=1-2
```

#### **응답 예시** (건물 조회):

```json
{
  "id": 1,
  "name": "Engineering Building A",
  "image": "<base64 image>",
  "classrooms":[
    {
      "id": 1,
      "name": "강의실 105동",
      "floor": 1,
      "code": "105"
    }
  ]
}
```

#### **응답 예시** (모든 건물 목록 조회):

```json
[
  {
    "id": 1,
    "name": "Engineering Building A"
  },
  {
    "id": 2,
    "name": "Science Hall"
  }
]
```

### `POST /add_building`

- **설명**: 새 건물을 등록합니다.

#### **요청 예시**

```json
{
  "name": "New Building"
}
```

#### **응답 예시**:

```json
{
  "message": "Building New Building added!"
}
```

### `POST /add_classroom`

- **설명**: 특정 건물에 강의실을 추가합니다.

#### **요청 예시**

```json
{
  "building_id": 1,
  "name": "303호",
  "floor": 3,
  "code": "303"
}
```

#### **응답 예시**:

```json
{
  "message": "Classroom 303호 added under Engineering Building A!"
}
```

## 경로 탐색 API

### `POST /navigate 또는 GET /navigate`

- **설명**: 시작점과 도착점 사이의 최단 경로를 반환합니다. 공사중인 길은 자동으로 제거되며 대체 경로를 탐색합니다.

#### **요청 예시**

```json
{
  "start": "Engineering Building A/101호",
  "end": "Science Hall/201호"
}
```

#### 응답 예시:

```json
{
  "path": [
    "Engineering Building A/101호",
    "Engineering Building A/출입문",
    "Science Hall/출입문",
    "Science Hall/201호"
  ],
  "distance": 137.5,
  "time": "약 2분",
  "status": "현재 사용할 수 없는 경로 제거됨"
}
```

#### 오류 또는 불가능한 경우:

```json
{
  "status": "도달 가능한 경로가 없습니다.",
  "path": [],
  "distance": "N/A",
  "time": "N/A"
}
```

## 간선 상태 관리 API

### `POST /edge_status`

- **설명**: 특정 간선(지점 간 경로)의 상태를 차단하거나 해제합니다.

#### **요청 예시**

```json
{
  "from": "Engineering Building A/101호",
  "to": "Engineering Building A/출입문",
  "blocked": true
}
```

#### 응답 예시:

```json
{
  "message": "Edge status updated successfully"
}
```

## 자동완성 API

### `GET /autocomplete`

- **설명**: 자동완성 검색 후보를 반환합니다.
  `?q=검색어&target=classes|buildings` 형식을 사용합니다.

#### **요청 예시**

```http
GET /autocomplete?q=전자&target=buildings
```

#### 응답 예시:

```json
[
  {
    "type": "building",
    "id": "1",
    "name": "전자정보대학관"
  },
  {
    "type": "classroom",
    "id": "1",
    "name": "이성원교수님연구실",
    "code": "LAB123",
    "floor": 2,
    "building": "전자정보대학관"
  }
]
```

## 파일 업로드 및 다운로드 API

### `POST /upload`

- **설명**: CSV 파일 업로드 (건물/교실/경로 정보 등록용)

#### **요청 형식**: `multipart/form-data`

#### **요청 필드**

- file: CSV 파일

#### 응답 예시:

```json
{
  "message": "File processed successfully",
  "path": "/statics/images/map.csv"
}
```

### `GET /files/<file_id>`

- **설명**: 저장된 파일을 다운로드합니다.

#### **예시 요청**

```http
GET /files/5
```

### Tips (건물 팁)

#### `GET /tips`
- 설명: 건물 ID에 해당하는 팁들을 조회합니다. building_id=0이면 공통 팁을 조회합니다.

- 쿼리 파라미터
    - **building_id**: int (옵션)

```json
[
  {
    "id": 1,
    "title": "엘리베이터 위치",
    "content": "건물 중앙에 있어요.",
    "link": "https://...",
    "building_id": 3
  }
]
```

## 파일 업로드

### `POST /upload_files`
- 설명: 게시글에 이미지 파일을 업로드합니다.

- **쿼리 파라미터**
    - **post_id**: int

- **요청 형식**: multipart/form-data

```json
{
  "message": "파일 업로드 성공!",
  "path": "/statics/images/1/example.png"
}
```

## 게시글

### `POST /posts/add`
- 설명: 게시글을 추가합니다.

#### 요청 예시

```json
{
  "title": "게시글 제목",
  "content": "내용",
  "building_id": 2
}
```

#### 응답 예시

```json
{
  "message": "Post '게시글 제목' created!",
  "id": 5
}
```

### `GET /posts`
- 설명: 게시글 목록을 조회합니다.

- 쿼리 파라미터
    - building_id: int (옵션)

#### 응답 예시
```json
[
  {
    "id": 1,
    "title": "제목",
    "content": "내용",
    "building_id": 2,
    "created_at": "2025-04-07 13:23:00",
    "images": ["/files/3", "/files/4"],
    "comments": [
      {
        "id": 1,
        "content": "댓글 내용",
        "created_at": "2025-04-07 13:25:00"
      }
    ]
  }
]
```

### `PUT /posts/update`

- 설명: 게시글을 수정합니다.

- 쿼리 파라미터
    - post_id: int

#### 요청 예시

```json
{
  "title": "수정된 제목",
  "content": "수정된 내용",
  "building_id": 3
}
```

#### 응답 예시

```json
{
  "message": "Post '수정된 제목' updated!"
}
```

### `DELETE /posts/delete`
- 설명: 게시글을 삭제합니다.

- 쿼리 파라미터
    - post_id: int

#### 응답 예시
```json
{
  "message": "Post '삭제된 제목, post_id: 3' deleted!"
}
```

## 댓글

### POST /comments/add
- 설명: 게시글에 댓글을 추가합니다.

#### 요청

```json
{
  "post_id": 1,
  "content": "이건 진짜 유용하네요!"
}
```

#### 응답 예시

```json
{
  "message": "댓글 추가 성공!",
  "id": 4
}
```

#### 응답: 해당 파일이 존재할 경우 파일 다운로드, 없을 경우 404 반환.

## 테스트용 페이지

### `GET /test-autocomplete`

- **설명**: 자동완성 테스트 페이지 HTML을 렌더링합니다.

### `GET /test-upload`

- **설명**: 파일 업로드 테스트 페이지 HTML을 렌더링합니다.
