---
title:  "zsh에 대하여.."
excerpt: "zsh란 무엇이고 oh-my-zsh를 이용해 맥의 bash 터미널 환경을 여러분의 입맛에 맞게 또 사용하기 쉽게 커스터마이징 해보세요"
layout: single
classes: wide
categories:
  - WEB
tags:
  - Canvas, HTML, CSS
---

# Canvas

CanvasAPI는 JavaScript와 HTML의 \<canvas\> 엘리먼트를 통해 그래픽을 그리기 위한 수단을 제공합니다. `에니메이션`, `게임 그래픽`, `데이터 시각화`, `사진 조작 및 실시간 비디오 처리`를 위해 사용됩니다.

Canvas 판에 그려지는 데이터 자체가 픽셀 데이터 하나하나를 조작할수 있기 때문에 일반 이미지나 CSS로는 표현할수 없는것을 가능하게 할수있습니다.

  
### Canvas 와 SVG

SVG와 Canvas 모두 그래픽을 표현하고 예전 브라우저에서 둘다 지원하지 않았지만 HTML5부터 많은 브라우저들이 지원하기 시작했습니다.


|  | Canvas | SVG |
|---|:---:|---:|
| 특징 | 비트(bit) | 벡터(vector) |
|  | 요소 자신을 기준으로 배치 |  |
|  | 위치 상 부모(조상)요소를 기준으로 배치 |  |
|  | 브라우저 창을 기준으로 배치 |  |

Pixel로 데이터를 이루는 Canvas와는 달리 SVG는 점과 점사이의 계산으로 이루어진 파일이기때문에 크기가 아무리 커져도 이미지가 흐려지거나 하지 않습니다. 