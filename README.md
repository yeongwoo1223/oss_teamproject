# findmusic

흥얼거리거나 따라 부른 **녹음 파일 하나로 노래를 찾고 싶은 사람**을 위한 Python 멜로디 전처리·검색 라이브러리입니다.

---

## 1. What it does

노래 제목이 기억나지 않을 때 멜로디를 흥얼거리면, 미리 만들어 둔 멜로디 DB와 비교해 가장 비슷한 곡을 찾아 줍니다.

| 단계 | 하는 일 |
|---|---|
| 1. 입력 | 사용자가 녹음한 음성 파일(WAV/MP3)을 읽음 |
| 2. 멜로디 추출 | 가장 두드러진 단일 음만 남겨 멜로디 시퀀스를 만듦 |
| 3. 정규화 | 사람마다 다른 음역대(키)와 부르는 속도 차이를 제거 |
| 4. 비교 | DB의 원곡 멜로디와 DTW 거리를 계산해 일치율 점수로 변환 (노래 일부만 불러도 매칭) |
| 5. 결과 | 일치율이 가장 높은 곡의 **곡명, 아티스트, 일치율, 관련 URL**을 반환 |

**대상 사용자**
- 지나가다 들었거나 오래전에 들은 노래를 찾고 싶은 사람
- 자신이 만든 곡과 비슷한 노래가 있는지 확인하고 싶은 작곡가

**범위 밖**: 실시간 마이크 스트리밍 입력, 멜로디 없는 가사 검색, 음원 스트리밍·다운로드, DB 실시간 연동

자세한 내용은 [프로젝트 제안서](docs/proposal.md)를 참고하세요.

## 2. Installation

Python **3.10 이상**이 필요합니다. 가상환경 사용을 권장합니다.

```bash
python -m venv .venv
source .venv/bin/activate          # Windows(Git Bash): source .venv/Scripts/activate
pip install -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ findmusic==0.1.0
```

설치 확인:

```bash
python -c "import findmusic; print(findmusic.__all__)"
```

> 현재는 **TestPyPI**(테스트 서버)에 배포된 버전입니다. 정식 PyPI 배포 후에는 `pip install findmusic` 한 줄로 설치할 수 있습니다.

## 3. Quick start

**지금 실행할 수 있는 것** (v0.1.0)

```python
import findmusic

print(findmusic.__all__)    # 공개 함수 목록
help(findmusic.core)        # 각 함수의 docstring 확인
```

**구현 후 사용 예시** (예정, 함수 이름은 바뀔 수 있음)

```python
from findmusic import load_audio, extract_melody, normalize_melody, rank_songs

y, sr = load_audio("hum.wav")                         # 1. 녹음 파일 읽기
query = normalize_melody(extract_melody(y, sr))       # 2~3. 멜로디 추출, 정규화
results = rank_songs(query, database="songs/", top_k=3)   # 4. DB와 비교
print(results[0])   # {'title': ..., 'artist': ..., 'score': 92.4, 'url': ...}
```

## 4. Dependencies

(예시)`pyproject.toml`의 `dependencies`와 같은 내용입니다.

| 패키지 | 용도 | 라이선스 | 상태 |
|---|---|---|---|
| (없음) | v0.1.0은 표준 라이브러리만 사용 | - | 현재 |
| `numpy` | 멜로디 배열 계산 | BSD-3-Clause | 추가 예정 |
| `librosa` | 오디오 읽기, 음높이 추출(pYIN), DTW | ISC | 추가 예정 |
| `soundfile` | WAV/MP3 디코딩 | BSD-3-Clause | 추가 예정 |
| `pretty_midi` | 원곡 MIDI에서 멜로디 추출 | MIT | 추가 예정 |

모든 의존성은 MIT 라이선스와 함께 사용할 수 있는 허용적(permissive) 라이선스입니다. GPL 계열은 사용하지 않습니다.

## 5. Tests

TODO (later in this course)

## 6. Documentation

| 문서 | 링크 |
|---|---|
| 문서 사이트 | https://yeongwoo1223.github.io/oss_teamproject/ |
| API 레퍼런스 | https://yeongwoo1223.github.io/oss_teamproject/api/ |
| 프로젝트 제안서 | https://yeongwoo1223.github.io/oss_teamproject/proposal/ |
| 팀 규칙 | https://yeongwoo1223.github.io/oss_teamproject/team-rules/ |

함수별 설명은 **API 페이지**에 있습니다. `main` 브랜치에 머지될 때마다 GitHub Actions가 사이트를 자동으로 갱신합니다.

## 7. Release

TODO (later in this course)

## 8. Team

**oss_team08**

| 이름 | GitHub | 역할 |
|---|---|---|
| 김영우 | [yeongwoo1223](https://github.com/yeongwoo1223) | 팀장 |
| 김기민 | [knsino0471](https://github.com/knsino0471) | 팀원 |
| 반영석 | [bahnys1104erica-create](https://github.com/bahnys1104erica-create) | 팀원 |
| 신희찬 | [shinhchn](https://github.com/shinhchn) | 팀원 |
| 이호천 | [js71607957](https://github.com/js71607957) | 팀원 |

**협업 방식**: 이슈 → 브랜치 → PR → 리뷰어 2명 이상 승인 → 머지. 자세한 규칙은 [team-rules.md](docs/team-rules.md)를 참고

**AI 사용**: AI 도구를 사용할 수 있으며, 모든 PR의 `AI use` 항목에 **도구, 사용 위치(파일/함수), 방식(생성/자동완성/리팩터링/설명/리뷰), 사후 조치**를 기록; 사용하지 않았다면 `None`이라고 적습니다.

## 9. License

[MIT License](LICENSE) © 2026 oss_team08
