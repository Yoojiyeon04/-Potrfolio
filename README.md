weather-app-project/
├── data/               # (선택) 과거 날씨 기록 저장용 CSV 등
├── src/                # 소스 코드 폴더
│   ├── main.py         # 프로그램 실행 메인 파일
│   ├── api_handler.py  # API 호출 및 JSON 데이터 파싱 로직
│   ├── ui.py           # GUI 관련 코드 (Tkinter/Streamlit)
│   └── utils.py        # 단위 변환, 알림 기능 등 공통 함수
├── assets/             # 날씨 아이콘, 배경 이미지, 데모 GIF
├── requirements.txt    # 필요한 라이브러리 목록 (requests, pandas 등)
├── .gitignore          # API 키 유출 방지를 위한 설정 (.env 등)
└── README.md           # 프로젝트 설명서 (가장 중요!)
