---
title: 온라인 호스팅 지침
permalink: index.html
layout: home
---

# 콘텐츠 디렉터리

각 사례 연구에 대한 하이퍼링크는 아래와 같습니다.


## 2023년 5월 콘텐츠 새로 고침을 위해 재구성된 사례 연구

{% assign casestudy= site.pages | where_exp:"page", "page.url contains '/Instructions/CaseStudyv2/'" %}
| 모듈 | CaseStudy |
| --- | --- | 
{% for activity in casestudy  %}| {{ activity.casestudy.module }} | [{{ activity.casestudy.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


## 이전 사례 연구 조직

{% assign casestudy= site.pages | where_exp:"page", "page.url contains '/Instructions/CaseStudy/'" %}
| 모듈 | CaseStudy |
| --- | --- | 
{% for activity in casestudy  %}| {{ activity.casestudy.module }} | [{{ activity.casestudy.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}