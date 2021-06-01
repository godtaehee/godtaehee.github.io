---
title:  "[JavaScript] Window객체"
excerpt: "Window객체와 BOM에 대해서 알아봅니다."
layout: single
classes: wide
categories:
  - JavaScript
tags:
  - JavaScript, JS, Front
---

# Window 객체

window객체는 브라우저의 요소들과 자바스크립트 엔진, 모든 변수들을 담고 있는 객체입니다.

브라우저의 위에는 즐겨찾기, 탭, 닫기버튼, 최소최대화 버튼등이 있고 그 밑에부터 실제 웹사이트가 표시되는데 이때 저런 버튼들과 웹사이트를 통틀어서 브라우저 전체를 담당하는 객체가 `window`객체 이고 웹사이트만을 담당하는 객체는 `document`입니다. window 아래에 있는 다양한 객체중에 하나가 `document`객체 입니다. 

window 아래에는 `screen`, `location`, `history`, `document`와 같은 객체들이 있습니다. 

window는 모든 객체의 조상인데 이를 `전역객체`라고 합니다. 따라서 브라우저 내의 모든 객체를 포함하고있으므로 window.screen, window.location, etc... 이라고 하지않고 그냥 screen, location으로 바로 사용가능합니다. 

## window.alert()

