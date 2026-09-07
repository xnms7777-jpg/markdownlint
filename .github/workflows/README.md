# 22.22wx
 import requests  URL = "http://127.0.0.1:8888/api" TOKEN = "..."  headers = {     "Authorization": f"token {TOKEN}" }  response = requests.get(URL, headers=headers)  print("Status:", response.status_code) print(response.text)
Visual Studio Code documentation search