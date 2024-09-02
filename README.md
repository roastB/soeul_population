# soeul_population_project

▶ 서울 생활인구 정규화(normalization)된 데이터를 통해 DataFrame으로 저장한 후, folium.Choropleth 이용하여 자치구별 서울 총생활인구중 내/외국인생활인구 파악하자.👀

▶ 이후 서울시 단기체류 외국인생활인구 및 유명 프랜차일드 위치 파악하여 상권 분석에 사용.🍔🍟


## Keyword Code

```python

# folium을 이용하여  자치구별 Map 정보가져오기
# (2) geo data -> Lucy Park 선생님의 서울맵을 github에서 불러와 사용.

import folium
import json

soeul_pop = pd.read_csv("/content/drive/MyDrive/colab_ws/data/seoul_pop_norm.csv")
soeul_pop.drop(index=0, axis=0, inplace=True)

geo_path = "/content/drive/MyDrive/colab_ws/data/02. skorea_municipalities_geo_simple.json"
geo_str = json.load(open(geo_path, encoding="utf-8"))

```

```python

# [1]"자치구"별 서울 총생활인구중 내국인생활인구

seoul_m = folium.Map(location = [37.5502,126.982], zoom_start = 11.4)

# Choropleth map
folium.Choropleth(
    geo_data = geo_str, # geo_struct의 줄임말
    data = soeul_pop,
    columns = ["Gu", "Local_norm"],
    key_on = "properties.name",
    fill_color = "PuRd",
    fill_opacity = 0.7,
    line_opacity = 0.2,
    legend_name=""" "자치구"별 서울 총생활인구중 내국인생활인구 "비율" """
    ).add_to(seoul_m)

seoul_m

```
<img width="100%" src="https://private-user-images.githubusercontent.com/37838530/363528773-23ac5655-9bde-4927-b93c-348d8451db2d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjUyNDk4NzIsIm5iZiI6MTcyNTI0OTU3MiwicGF0aCI6Ii8zNzgzODUzMC8zNjM1Mjg3NzMtMjNhYzU2NTUtOWJkZS00OTI3LWI5M2MtMzQ4ZDg0NTFkYjJkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA5MDIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwOTAyVDAzNTkzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZjYzNmZWRhZmNjNzM3YmU3OTZiMzRmMmU0MDMyMThjNzEyZDU1Yjg3NmFhOWRhMzkyMWFkZjQzZDdkOWU2ODgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.lspVup6H_XjbI3q4Su4NqFD7Qx5_u2O2gn4-MyTZ7tM">

```python

# [2]"자치구"별 서울 총생활인구중 외국인생활인구(장기체류+단기체류)

seoul_m = folium.Map(location = [37.5502,126.982], zoom_start = 11.4)

# Choropleth map
folium.Choropleth(
    geo_data = geo_str, # geo_struct의 줄임말
    data = soeul_pop,
    #참고 : https://python-visualization.github.io/folium/latest/getting_started.html
    columns = ["Gu", "total_Foreign_norm"],
    key_on = "properties.name",
    fill_color = "PuRd",
    fill_opacity = 0.7,
    line_opacity = 0.2,
    legend_name=""" "자치구"별 서울 총생활인구중 내국인생활인구 "비율" """
    ).add_to(seoul_m)

seoul_m

```
<img width="100%" src="https://private-user-images.githubusercontent.com/37838530/363528580-b5760365-f435-48fd-aff0-6900fcbea2da.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjUyNTAwNzMsIm5iZiI6MTcyNTI0OTc3MywicGF0aCI6Ii8zNzgzODUzMC8zNjM1Mjg1ODAtYjU3NjAzNjUtZjQzNS00OGZkLWFmZjAtNjkwMGZjYmVhMmRhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA5MDIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwOTAyVDA0MDI1M1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWRkMWYzYjNlOTExY2NmNzZmZWJmMmNkZWM4ZWI0NWEyYjQ5ZmFlMzBmMjEyOGFiOGU2NThhZmE4MDk5NmE5ZTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.pGHE6ZWrL33XJznxR_Qn6Roz9TCh-U-bXmwuaJ1o7J8">


## References

Data : [자치구단위 서울생활인구 일별 집계](https://data.seoul.go.kr/dataList/OA-15379/S/1/datasetView.do)
folium : [Homepage](https://python-visualization.github.io/folium/latest/getting_started.html)



