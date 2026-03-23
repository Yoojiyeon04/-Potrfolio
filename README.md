import requests
import sqlite3
from datetime import datetime

# 1. 데이터베이스 설정 (SQLite)
def init_db():
    conn = sqlite3.connect('weather_data.db')
    cursor = conn.cursor()
    # 날씨 정보를 저장할 테이블 생성
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS weather_history (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            city TEXT,
            temp REAL,
            description TEXT,
            humidity INTEGER,
            date TEXT
        )
    ''')
    conn.commit()
    return conn

# 2. 날씨 API 데이터 가져오기 (OpenWeatherMap 기준)
def get_weather(city_name, api_key):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={api_key}&units=metric&lang=kr"
    response = requests.get(url)
    
    if response.status_code == 200:
        return response.json()
    else:
        print("데이터를 가져오는 데 실패했습니다.")
        return None

# 3. DB에 데이터 저장하기
def save_to_db(conn, city, data):
    cursor = conn.cursor()
    now = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    
    temp = data['main']['temp']
    desc = data['weather'][0]['description']
    hum = data['main']['humidity']
    
    cursor.execute('''
        INSERT INTO weather_history (city, temp, description, humidity, date)
        VALUES (?, ?, ?, ?, ?)
    ''', (city, temp, desc, hum, now))
    
    conn.commit()
    print(f"[{now}] {city}의 날씨 정보가 DB에 저장되었습니다.")

# --- 메인 실행부 ---
if __name__ == "__main__":
    API_KEY = "본인의_API_키를_입력하세요"  # OpenWeatherMap에서 발급받은 키
    CITY = "Seoul"
    
    db_conn = init_db()
    weather_json = get_weather(CITY, API_KEY)
    
    if weather_json:
        save_to_db(db_conn, CITY, weather_json)
        
        # 저장된 데이터 확인용 출력
        print(f"현재 온도: {weather_json['main']['temp']}°C")
        print(f"날씨 상태: {weather_json['weather'][0]['description']}")

    db_conn.close()
