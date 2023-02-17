---
layout: post
title: "YOLO V5를 이용한 개인형 이동장치 이용자 헬멧 착용 유무 확인 프로그램"
---

# 분석 배경
개인형 이동장치와 이륜차의 이용량이 증가하면서 사고 역시 동시에 증가하는 추세이고 헬멧 미착용 시 사망 사고 건수의 비율이 3배 이상으로
안전에 큰 영향이 있는 것을 확인하여 프로그램 개발을 시작함

# 분석 방법
- 이미지 데이터를 수집하였지만 데이터 양과 질이 부족하여 Data Augmentation 진행 - Roboflow로 진행
1. Ratation(-15°~+15°)
2. Hue(-25°~+25°)
3. Saturation(-25°~+25°)
4. Brightness(-25°~+25°)
5. Exposure(-15°~+15°)
6. Noise(픽셀의 최대 3%)

- 객체 별 클래스 구분
·헬멧을 착용하지 않은 개인형 이동장치 이용자(helmet)
·헬멧을 착용한 개인형 이동장치 이용자(non_helmet)

# 분석 결과
성능은 높지 못한 것으로 나옴, 따라서 파라미터 설정과 데이터의 다양성을 추구하여 성능 향상 필요!
때때로 헬멧 착용 이용자와 헬멧 미착용 이용자를 인식하지 못하거나 다르게 인식하는 경우가 존재...


# YOLO V5 실행 CODE
[agumentaion.md](https://github.com/suljihoon/suljihoon.github.io/files/10763961/agumentaion.md)
