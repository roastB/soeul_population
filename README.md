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
[<img src = "https://teddylee777.github.io/images/2021-11-08/image-20211108010809526.png">](https://github.com/roastB/soeul_population/issues/2#issue-2499770087)

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
https://github.com/roastB/soeul_population/issues/3#issue-2499772141


## References

Data : [자치구단위 서울생활인구 일별 집계](https://data.seoul.go.kr/dataList/OA-15379/S/1/datasetView.do)
folium : [Homepage](https://python-visualization.github.io/folium/latest/getting_started.html)



