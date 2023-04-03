# ClassicalChinese Translation of ancient documents

Main project2






## 

## 고문서 한자 OCR & 번역
<br>

---

### 목   차

1. 프로젝트 기획(Proposal)
2. 전처리(Preprocessing)
3. 모델 학습(Train Model)
4. 서비스 구현(Implement)
5. 결과 및 피드백(FeedBack)

<br>

---
## 1. 프로젝트 기획

<br>

### **1-1. 프로젝트 주제**


고문서 이미지의 **한자**를 **OCR**을 통해 추출하고, **정렬**한 뒤 번역기를 통과시켜서 **번역**문을 출력

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092341451020578876/image.png?width=1168&height=558" width="680">



<br>
<br>


### **1-2. 프로젝트 목적**

(1) 프로젝트 기획 목적
+ **고문서**는 정치·경제·사회·문화·예술·과학·의학·사상·종교 등 다방면에서 민족의 과거와 지식을 담고 있는 매우 중요한 기록유산.
+ 현재 **OCR**을 활용한 고문서 **텍스트 추출·변환 기술이 미비**하여 한자 콘텐츠에 대한 검색 및 분석 등의 **활용이 어려움**


(2) 프로젝트 가치
+ 생성된 모델을 통해 고문서를 해석학시 위해 필요한 **전문 인력을 대체** 할 수 있음.
+ 해석된 문서를 통해 여러 분야에서 현대적으로 재해석된 **콘텐츠 개발 시 이용 가능** 
+ **역사 연구 및 한문 학습** 시 이용 가능

<br>
<br>

### **1-3. 팀 구성 및 역할**
- 수행 인원 : 2명
- 팀 구성원 역할<br>
  + 황성연(팀장) : OCR 데이터 전처리, OCR 모델 구현, 번역 모델 구축, PPT 작성 및 발표<br>
  + 이정우 : 번역 데이터 수집, OCR 모델 핸들링, 번역 모델 핸들링, 웹 페이지 작성<br>
  (분담한 역할에 한정되지 않고 서로 유기적으로 협력하며 진행)

(2) 프로젝트 가치
+ 생성된 모델을 통해 고문서를 해석학시 위해 필요한 **전문 인력을 대체** 할 수 있음.
+ 해석된 문서를 통해 여러 분야에서 현대적으로 재해석된 **콘텐츠 개발 시 이용 가능** 
+ **역사 연구 및 한문 학습** 시 이용 가능

<br>
<br>

### **1-4. 수행 기간**

<img src='https://media.discordapp.net/attachments/1002189622912221250/1092340776798785546/image.png?width=1147&height=617' width=680>

<br>
<br>


---
## 2. 전처리

### **2-1. 데이터 분석**
OCR 모델을 위한 데이터, 번역 모델을 위한 데이터 각각 분석

(1) OCR 데이터 <br>
수집 방법 : 'AI HUB'의 **'고서 한자 인식 OCR'** 데이터

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092342586703892500/image.png?width=1013&height=620" width="680">

<br>

(2) 번역 데이터 <br>
수집 방법 : '국사편찬위원회' 사이트를 활용해 수기 작성 + '동양고전종합DB' 사이트 크롤링

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092343249500389537/image.png?width=1037&height=515" width="680">

<br>


### **2-2. OCR 데이터 전처리**

(1) COCO 형식의 어노테이션 데이터를 YOLO 형식으로 변경  <br>
학습 할 모델이 YOLO 모델이기 때문에, 그에 맞춰 어노테이션의 포맷을 변경해줌.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092343822308085801/image.png?width=1201&height=515" width="680">

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092343983545516042/image.png?width=1117&height=617" width="680">


<br>

(2) 이미지 Resize & Padding <br>

컴퓨터 자원의 한계로 인해 크기가 큰 이미지학습은 불가능하기 때문에, 사이즈를 조금씩 줄여가며 학습이 가능한 선까지 크기를 축소함.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092344209194876958/image.png?width=1022&height=461" width="680">


<br>

(2) 상하단 여백 Crop을 통해 이미지 크기 최적화 작업 <br>

상·하단의여백을 잘라냄으로써 바운딩박스의 비율을 늘려 크기가 작은 글자를 좀 더 잘 잡아낼 수 있도록 함.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092344498102739005/image.png?width=900&height=492" width="680">

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092344647239614554/image.png?width=852&height=423" width="680">



<br>


### **2-3. 번역 데이터 전처리**

(1) 번역문의 길이가 지나치게 긴 샘플 제거 <br>

길이가 지나치게 긴 문장으로 인해 전체 문장의 패딩크기가 커지면 학습 시간이 오래 걸리고, 사용 가능한 자원으로 학습이 힘듬.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092345171988975646/image.png?width=1070&height=512" width="680">

<br>

(2) 노이즈를 포함한 문장 수정 및 제거 <br>
노이즈가 포함된 데이터로 인해 발생하는 오역을 줄이기 위한 작업.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092345360556490782/image.png?width=838&height=517" width="680">

<br>

(3) 한문, 국문 토크나이저 생성 및 토큰화 작업 <br>

토큰화 작업 후 숫자로 변환하는 임베딩 작업을 수행함.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092345570313637888/image.png?width=1066&height=562" width="680">


<br>
<br>


---
## 3. 모델학습

<br>

### **3-1. OCR 모델 학습**

(1) 모델 선정 : YOLO V4, V5, V6

YOLO 모델 선정 이유
+ 학습 파이프라인이 간단해 학습과 예측의 속도가 빠름
+ 이미지 전체를 한 번에 바라보기 때문에 class에 대한 맥락적 이해도가 높음
  -> 낮은 background error

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092352839071830087/image.png?width=1202&height=322" width="680">


<br>

(2) 학습에 활용한 데이터 수
+ 전체 데이터는 총 50,118개지만 컴퓨터 자원의 한계로 전체 학습은 불가능
+ 두 가지 Case 로 나눠서 데이터를 학습에 사용
  + 1번 데이터셋 : 학습 데이터의 여러 필체 중 깨끗한 '해서체'로 이뤄진 데이터 활용(29,046개)
  + 2번 데이터셋 :'해서체'로 이뤄진 데이터 중 10000개만 더 큰 사이즈로 학습(10,000개)


<br>

(3) 1번 데이터셋 학습 (29,046개 -> 사이즈는 500×500)

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092354430260760657/image.png?width=760&height=601" width="680">

상 하단 여백을 Crop하여 바운딩 박스 부분의 비율을 늘렸을 때 더 높은 성능 기록

<br>

(4) 2번 데이터셋 학습 (10,000개 -> 사이즈는 700×700)


<img src="https://media.discordapp.net/attachments/1002189622912221250/1092354610393534484/image.png?width=842&height=636" width="680">

1번 세트와 비교하여 사용한 데이터 수는 줄었지만, 학습하는 이미지의 크기를 늘림으로써 성능 상승 <br>
(한 이미지에 담긴 한자들은 사이즈가 작기 때문에, 작은 물체를 좀 더 잘 잡기 위해서는 기본적으로 이미지의 크기가 커야 하기 때문)

<br>

(5) YOLO 모델의 성능 향상을 위한 추가적인 시도
+ **Hyper Parmeter Tuning**(batch-size, lou Threshold, Momentum 등)
+ **Background Image**를 넣어 False Postive(FP) 줄이기 <br>
  (Background Image는 탐지 할 물체가 없는 이미지, 전체 학습 데이터 셋의 0~10% 정도 활용 -> Background Image 활용 시 False Postive 하락의 효과 기대)
+ **Pre Trained Weights** 사용<br>
  작은 크기의 데이터셋의 경우 좋은 효과를 기대할 수 있음

그러나 이런 여러 시도에도 불구하고 유의미한 성과를 거두지는 못함.

<br>

(6) 낮은 성능의 원인 분석
+ 지나치게 많은 Class 수
  + **Class의 종류**는 **12,763**개나 될 정도로 많음.
  + Object Detection이 아닌 단순 분류 모델에서도 쉽지 않은 다중 분류 문제
+ 이미지의 지나친 축소
  + 하나의 이미지에는 대략 100~400개의 많은 객체(한자)가 포함됨.
  + 모델을 돌릴 수 있을 정도로 **사이즈**를 **축소**(원본은 2000×3000이지만 축소한 사이즈는 700×700)하다 보니 **객체의 크기가 너무 작아 짐.**
+ YOLO Model의 단점
  + 속도가 빠르고 전체적으로 mAP가 높지만 **작은 객체에 대한 정확도가 낮은 단점**을 가지고 있음.


<br>

(7) 사전 학습된 모델 활용 <br>
HRCenterNet & ResNet
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092357771338453092/image.png?width=1102&height=583" width="680">

<br>
<br>

### **3-2. 번역 모델 학습**

(1) 모델 선정 : LSTM, GRU, Bidirectional LSTM 등의 RNN 계열 / Transformer

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092358155050176563/image.png?width=921&height=541" width="680">

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092374968496554024/image.png?width=923&height=446" width="680">



<br>

(2) 모델 평가 방법
+ 학습 중 Validation 평가 : Masked Accuracy, Masked Loss <br>
  + 단순 Accuracy, Loss가 아닌 **Padding Masked 된 값을 제외**하고 Accuracy와 Loss를 측정
+ 학습 후 Testdata 평가 : BLEU Score, 직접 오역 여부 확인
  + BLEU Score : **N그램 기반 기계 번역 결과**와 **사람이 직접 번역**한 결과가 **얼마나 유사**한지 비교하여 번역에 대한 성능을 측정하는 방법
  + 수치로만이 아닌 눈으로 직접 번역 결과를 확인
  
<br>

(3) 모델 학습 (RNN 계열 모델)

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092375428313923655/image.png?width=952&height=402" width="680">

전체적으로 낮은 성능 -> Inference 결과 제대로 된 문장을 출력하지 못함

<br>

(4) 성능 향상을 위한 추가적인 시도와 결과 분석 (RNN 계열 모델)

+ 성능 향상을 위한 추가적인 시도
  + 형태소 분석기 변경 : Mecab, Okt, 다양한 Bert 계열의 사전학습된 Tokenizer 사용
  + 기타 Hyper Parameter Tuning(Optimizer, DropOut, Kerel_initializer)
  
  -> 크게 유의미한 성과를 거두지는 못합
+ 낮은 성능의 원인 분석
  + LSTM, GRU를 통해 장기 의존성 문제를 어느 정도는 해결할 수 있지만, **길이가 지나치게 긴 한문**의 경우 **순서 정보**가 제대로 **반영되지 못하고** 장기 의존성 문제 해결에 한계가 있음
  + **한자**는 **단어 하나하나**에 의미를 담고 있는 경우가 많음. 반면 **한국어**는 음소문자로 소리를 상징하는 **자음과 모음이 모여 단어의 의미를 형성**. 기본적인 구조의 차이에서 오는 Task의 어려움.

<br>

(5) 모델 학습 (Transformer)

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092377067062050896/image.png?width=1075&height=523" width="680">

<br>

(6) 성능 향상을 위한 추가적인 시도와 결과 분석 (Transformer)
+ 성능 향상을 위한 추가적인 시도
  + 데이터 추가 : 초기 데이터는 4~5만개 정도, 낮은 성능을 향상시키기 위해 **지속적으로 데이터 추가**
  + 데이터 선별 작업 : 데이터의 **노이즈 제거**, 길이가 지나치게 긴 샘플 제거 등 데이터를 선별하여 사용.
  + 고유명사를 구분해주기 위한 시도
    + 번역 데이터에서 고유명사는 **괄호와 한자를 포함**하고 있음 : ex) "재물은 3대(代)를 못 간다"
    + 괄호 속 한자를 고유명사 인식의 지표로 사용할 수 있을 것으로 추정하고 **포함 했을때와 미포함했을 때 결과 비교** -> 포함했을 때 더 좋은 결과

  -> 전체적으로 모델보다는 데이터 핸들링의 영향을 많이 받음
+ 결과 분석
  + 난이도 자체가 높은 Task와 데이터의 순수성, 규모의 한계로 인해 모델의 완성도를 높이기 어려움.
  + Hyper Parameter Tuning의 한계점 존재
    + 모델의 Hyper Parameter Tuning은 전체적으로 어느 정도 성능이 나오고, 학습 데이터에 과적합된 상황에서 모델을 규제하며 검증 데이터에 대한 정확도를 높이는 방향일 때 많은 도움을 줄 수 있지만, Underfitting 문제 자체를 해결하는데에는 한계가 있다는 판단.

<br>

(7) Inference 결과

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092379261182156830/image.png?width=997&height=640" width="680">



---
## 4. 서비스 구현

<br>

### **4-1. HTML 기반 웹 페이지 작성**

(1) HTML, CSS, Java Script를 활용하여 서비를 구현하기 위한 웹 페이지 생성

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092379600216145990/image.png?width=1008&height=521" width="680">


<br>

### **4-2. Flask를 활용해 서버와 클라이언트간의 정보 전달**

(1) Flask를 통해 클라이언트로부터 입력 받은 이미지를 저장 후 **Super Resolution**을 이용한 이미지 확대 적용<br>
(너비와 높이 중 낮은 쪽이 500~1000 크기면 2배 / 그 이하는 4배 적용)

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092380130896252928/image.png?width=1131&height=502" width="680">

(2) OCR 모델을 활용해 글자 추출 후 Sorting 한 뒤, Flask를 통해 클라이언트에 전달
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092380634770579546/image.png?width=1016&height=512" width="680">

(3) 전달한 추출 문장, 혹은 사용자가 직접 입력한 문장을 Flask를 통해 받아와 한글로 번역 후 클라이언트에 전달
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092381114200498217/image.png?width=1058&height=556" width="680">


- 번외 - 네이버 파파코와 번역 결과 비교
  
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092381769745051659/image.png?width=1008&height=741" width="680">


---
## 5. 결과 및 피드백

<br>

### **5-1. OCR 모델 결과 피드백**

(1) 직접 학습(YOLO V4, V5, V6)
+ 결과
  + 다양한 시도를 통해 모델 성능을 끌어올리려고 시도했지만 아쉬운 결과
+ 원인 분석
  + 지나치게 많은 클래스의 수, 자원의 한계로 인한 이미지 사이즈 축소, YOLO 모델이 가지는 한계점(작은 객체를 잘 잡아내지 못함)
+ 배운 점
  + Detercion Annotation Format과 이미지 처리에 관한 이해
  + 사이즈를 줄여도 객체가 작아지지 않도록 할 수 있는 방법에 대한 고민
  + Object Detection Model에 대한 전반적인 학습
  
(2) 사전 학습 모델 상용(HRCenterNet, Resnet)
+ 결과
  + 학습이 잘 되어있는 모델이기에 Inference시 우수한 성능을 보임
+ 원인 분석
  + 풍부한 데이터와 좋은 성능의 개발 환경속에서 오래 학습했기에 좋은 결과를 보여주는 것으로 추정
+ 배운 점
  + B-Box를 원하는 방식으로 Sorting 하는 방법에 대한 고민과 학습
  + 다소 학습 경험이 부족한 2 Stage Model에 대한 이해

<br>
<br>

### **5-2. 번역 모델 결과 피드백**

(1) RNN 계열 모델(LSTM, GRU, BILSTM)
+ 결과
  + 다양한 핸들링에도 불구하고 낮은 성능
+ 원인 분석
  + 기본적으로 문장의 길이가 긴 번역 데이터에서 순서 정보를 제대로 반영하지 못하는 장기 의존성 문제
+ 배운 것
  + RNN(Recurrent Neural Network) 모델에 대한 학습
  + 다양한 언어에 대한 전처리 방법


(2) Transformer 모델
+ 결과
  + RNN Model에 비해서 비교적 좋은 성능을 보이며 더 나은 Inference 결과를 보여줌
+ 원인 분석
  + 포지셔널 인코딩을 통한 순서 정보의 반영, 어텐션 메커니즘이 전체적으로 사용되어 단어간의 연관성이 잘 반영되기에 기계 번역 Task에 적합한 것으로 추정
+ 배운 것
  + Transformer 모델에 대한 이해와 번역 성능 향상을 높이기 위한 방법들


<br>
<br>

### **5-3. 전체 과정에 대한 피드백**

(1) 아쉬운 점
+ OCR Model 학습 시 시간 문제로 Two Stage Model을 시도해보지 못한 부분
+ 트랜스포머 구현 과정에서 지나치게 많은 시간을 소모함
+ 프로젝트 시간 조절의 아쉬움(각 단계가 계획했던 것보다 더 많은 시간이 소요됨)

(2) 긍정적인 점
+ 메인 1차 이미지, 텍스트 분류 프로젝트에 이어서 Object Detection, 기계 번역 모델 등 다양한 Task를 경험해봄
+ Object Detection, Translation 모델의 개선 가능성
+ 웹 서비스 구현 시 더 많은 기능 탑재와, 디자인 개선을 적용한다면 높은 가치를 가진 서비스를 만들 수 있을거로 생각.


