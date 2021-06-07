# 강화학습을 통한 포트폴리오 설계 (Capstone design 2021-1)
* 산업경영공학과 16학번 김수영

## Overview

* Goals, objectives (evaluation)
	1) Rule, 기술적 분석에 기반한 트레이딩 봇을 개발한다.
	2) 강화학습에 기반한 트레이딩 봇을 개발한다.
	3) Back-Testing을 통한 트레이딩 봇의 성능 평가 결과를 비교하여 제시한다.
	4) 강화학습을 성공적으로 수행하여, 실제 운용 가능한 수준의 트레이딩 봇을 개발한다.
## Schedule
|            Contents           | March | April |  May  | June  |   Progress   |
|-------------------------------|-------|-------|-------|-------|--------------|
|  강화학습에 대한 배경지식 확보  |   O   |       |       |       |     Link1    |
|  Nasdaq 개별 종목의 데이터 확보 |   O   |       |       |       |     Link2    |
|      데이터 셋 가공 및 구축     |   O   |       |       |       |     Link2    |
|   Rule 기반의 트레이딩 봇 개발  |       |   O   |    O    |       |     Link2    |
| 강화학습 기반의 트레이딩 봇 개발 |       |       |   O   |       |     Link2    |
| Back-Testing을 통한 봇 성능 평가|      |       |    O   |      O  |     Link2    |
|    봇 성능 개선 및 실제 운용    |     |       |       |    O    |     Link2    |

## Results
3. 수행결과
   가. 과제수행 결과
1. 데이터 셋 확보 

1.1 금융 데이터
향후 모델링 등에 필요한 금융 데이터는 1980년 1월 초부터 현 시점까지의 약 40년 분량의 금융 데이터를 수집하였다. Yahoo Finance, FRED API를 이용하여 수집하였고 그 내역은 아래와 같다.
1.2 금융 데이터
1) T10Y2Y : 미 국채 10년물과 2년물의 장단기 금리 차
2) NASDAQ: 첨단 벤처 기업들이 상장되어 있는 미국의 주식 장외 시장 지수 
3) DOWJONES : 국의 증권거래소에 상장된 30개의 우량기업 주식 종목들로 구성된 지수
4) GOLD : 일별 금 시세
5) DGS30 : 미 국채 30년물(무위험 수익 가정)


2. 강화학습 모델링
강화학습의 핵심은 일단 해 보는 것이다. 그 행동의 결과가 자신에게 유리한 것이었다면 상을 받고, 불리한 것이었다면 벌을 받는 것이다. 
이 과정을 매우 많이 반복하면더 많은 보상을 받을 수 있는 더 좋은 답을 찾아낼 수 있다는 것이 강화학습의 기본 아이디어이다.


![image](https://user-images.githubusercontent.com/44190559/120962135-1cfa7880-c79a-11eb-84bc-e5e8cf191497.png)
(출처 생활코딩, https://opentutorials.org/course/4548/28949)

이걸 그림으로 나타내면 위와 같다.
1. 우선 게임은 게이머에게 현재의 상태를 보여줍니다. 캐릭터는 어디에 있고, 장애물은 어디에 있는지 알려줍니다.
2. 동시에 현재의 점수도 알려줍니다. 게이머는 이 값이 높아지는 것이 상이고, 장애물에 부딪히는 것이 벌입니다.
3. 관찰의 결과에 따라서 어떤 상태에서 어떻게 행동해야 더 많은 상을 받고, 더 적은 벌을 받을 수 있는지를 알게 됩니다.
4. 즉, 판단력이 강화된 것입니다.
5. 판단에 따라서 행동을 합니다.
6. 그 행동은 게임에 변화를 주게 되며, 게이머의 실력은 점점 늘어납니다.

 위의 과정을 주식 투자에 대치시켜 강화 학습을 모델링할 수 있다.

2.1 상태


![image](https://user-images.githubusercontent.com/44190559/120962196-3a2f4700-c79a-11eb-9eb5-0d3660790733.png)


먼저 상태는 60일 간의 개별 종목의 주가 흐름과 거시경제 지표 중 하나인 장단기금리차이다. 5가지의 지표를 통해 AI는 상황을 판단하고 액션을 결정한다.

2.2 Action

![image](https://user-images.githubusercontent.com/44190559/120962213-41565500-c79a-11eb-8488-cbafc3ced10c.png)


처음에 AI는 아무것도 모르니 random하게 판단해 행동을 취한다. random하게 판단하는 비율은 시간이 지날수록 점점 낮아져 5%까지 낮아지게 설정하였으며, 행동은 포트폴리오의 비율을 조정하는 것이다. 행동 자체는 이산적으로 표현되어야 하기에 총 80가지의 경우를 만들었으며, 80가지 중 한 가지 포트폴리오를 취함으로써 ai는 다양한 시도를 할 수 있었다. 

2.3 Episode와 보상
 게임 한 판을 하는 걸 episode라고 일컫는데, 여기서는 한 episode 당 4번의 행동을 취한다. 즉 240일 동안 4 번의 포트폴리오 비중을 조정하며 수익률 변화를 관찰하는 것이다. 240일인 이유는 1년을 가정한 것인데, 주말과 공휴일을 빼고 주식 시장이 열리는 날을 고려한 값이다.
 
 
![image](https://user-images.githubusercontent.com/44190559/120962228-487d6300-c79a-11eb-9ba5-0f8d4c7a966e.png)


행동에 따른 보상은 수익이다. 투자의 결과를 그대로 보상으로 받으며, 시작 금액은 십만 달러로 설정했다. 보상을 받으며 ai는 정책을 강화시켜 나간다. 어떤 포트폴리오 비중을 가져야 수익률이 잘 나오는지 AI는 보상을 받으며 스스로 알아갈 것이다.


3. 강화학습 결과

3.1 수익률


![image](https://user-images.githubusercontent.com/44190559/120962253-50d59e00-c79a-11eb-87b2-e2e816dd0f5c.png)


50만 회를 학습시켰을 때 수익률을 도식화한 결과이다. 개별 수익률은 시장의 변화 상황에 따라 noise가 큰 모습을 보여주지만, 수익률의 평균인 주황색 선을 보면 학습이 될수록 점점 상승하는 모습을 보이고 있다.

3.2 선택한 전략

![image](https://user-images.githubusercontent.com/44190559/120962270-5c28c980-c79a-11eb-8f57-21e54bc21064.png)
![image](https://user-images.githubusercontent.com/44190559/120962273-5df28d00-c79a-11eb-8382-d2fc6a3b868a.png)

AI가 어떤 전략을 취했는지 확인해 볼 수 있다. 왼쪽의 그래프는 80가지의 전략 중 어떤 전략을 가장 많이 취했는지에 대한 결과이고, 오른쪽 그래프의 빨강, 파란 막대 그래프는 각각 하락장, 상승장에서 어떤 전략을 취했는 지 비교한 값이다. 그래프를 보시면 시장의 변화 상황에 대한 액션의 큰 차이는 없었다.
53번째 전략이 가장 비중이 높은데, 어떤 전략들이 비중이 높았는지 정렬해 보니 아래와 같았다.
![image](https://user-images.githubusercontent.com/44190559/120962284-65199b00-c79a-11eb-9e22-bf28d20c305f.png)

어떤 방식으로든 나스닥이나 다우와 같은 주식에 투자하는 것이 수익률을 높여준다고 판단을 한 것 같다.  이렇게 금, 채권 등의 안전자산에 아예 투자하지 않은 옵션이 전체의 15%를 차지하였습니다.
반면 거의 선택하지 않았던 옵션은 금에 투자하는 것이다. 전체 액션에서 약 1퍼센트를 차지하였는데, 원자재의 특성상 가격 변동이 거의 없는데, 그런 부분이 반영된 결과라 생각한다.

## Conclusion
본 연구에서는 ai 강화학습을 통해 주식을 자동으로 투자해보는 것을 경험하며 수익률이 학습에 따라 올라가는 것을 보기도 하였지만, 동시에 한계점도 느꼈다.
첫째로는 40년이라는 한정된 데이터 내에서 학습을 통해 수익률을 올려나갔기 때문에, 앞으로 알 수 없는 미래의 주식시장에선 어떤 결과를 보일 지 모른다는 점이다.
두 번째로는 risk를 고려하는 투자가 수행되지 않았다는 점이다. Ai는 과거 데이터를 지속적으로 보면서 어떤 시기에 어떤 주가가 나올지를 터득했기 때문에, 그냥 위험자산인 주식에 오롯이 자산을 투자하였다. 현실에서는 미래는 절대 알 수 없기 때문에 반드시 안전자산에 대한 투자가 병행되어야 한다. 따라서 졸업논문과 같은 다음 연구에서는 risk에 대한 고려가 병행되는 CAPM과 같은 모형을 강화학습에 응용하고자 한다. 

## Reports
* Upload or link (e.g. Google Drive files with share setting)
* Midterm: [Report](Reports/Midterm.pdf)
* Final: [Report](Reports/Final.pdf), [Demo video](Reports/Demo.mp4)
