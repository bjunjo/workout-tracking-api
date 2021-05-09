# workout-tracking-api
## Problem: Tracking workout using natural language processing and other APIs
## Solutions
1. Use an environment variable to seperate private information
```
APP_ID = os.environ["APP_ID"]
API_KEY = os.environ.get("APP_KEY")
SHEET_ENDPOINT = os.environ.get("SHEET_ENDPOINT")
TOKEN = os.environ.get("TOKEN")
USERNAME = os.environ.get("USERNAME")
PASSWORD = os.environ.get("PASSWORD")

```
2. Exercise data (natural language processing API)
```
sheet_headers = {
    "Authorization": TOKEN
}

exercise_endpoint = "https://trackapi.nutritionix.com/v2/natural/exercise"
exercise_headers = {
    "x-app-id": APP_ID,
    "x-app-key": API_KEY,
}

exercise_data = {
     "query": input("Tell me which exercises you did: "),
}

response = requests.post(url=exercise_endpoint, headers=exercise_headers, json=exercise_data)
response.raise_for_status()
```
3. Put this exercise data to Google Sheets
```
exercise_data = response.json()["exercises"]
today_date = datetime.now().strftime("%d/%m/%Y")
today_time = datetime.now().strftime("%H:%M:%S")

for exercise in exercise_data:
    sheet_inputs = {
        "workout": {
            "date": today_date,
            "time": today_time,
            "exercise": exercise["name"].title(),
            "calories": exercise["nf_calories"],
            "duration": exercise["duration_min"],
        }
    }
    sheet_response = requests.post(SHEET_ENDPOINT, json=sheet_inputs, headers=sheet_headers)
    print(sheet_response.text)
```    
