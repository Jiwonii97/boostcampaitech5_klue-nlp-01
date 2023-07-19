# level2_klue-nlp-01
해당 프로젝트는 네이버 커넥트재단 주최한 부스트캠프 AI Tech 5기 NLP 트랙 교육 과정 중 진행된 대회 프로젝트 입니다.  

---
### 랩업 리포트  

상세한 프로젝트 내용은 랩업 리포트를 참고해주세요!!  
[Wrap up](./KLUE_NLP_01_Wrap_Report.pdf)

---

<img src="https://github.com/Jiwonii97/boostcampaitech5_klue-nlp-01/assets/79522982/5a3bd3f1-4f96-4767-a536-a237677a6375" alt="drawing" width="500"/>

### A. 프로젝트 주제

<blockquote>
💡 관계 추출(Relation Extraction)

</blockquote>

- 문장의 단어(Entity)에 대한 속성과 관계를 예측하는 문제
- 지식 그래프 구축을 위한 핵심 구성 요소로, 구조화된 검색, 감정 분석, 질문 답변하기, 요약과 같은 자연어처리 응용 프로그램에서 중요하게 여겨짐
- 비구조적인 자연어 문장에서 구조적인 triple을 추출해 정보를 요약하고, 중요한 성분을 핵심적으로 파악할 수 있습니다
    
```
sentence: 오라클(구 썬 마이크로시스템즈)에서 제공하는 자바 가상 머신 말고도 각 운영 체제 개발사가 제공하는 자바 가상 머신 및 오픈소스로 개발된 구형 버전의 온전한 자바 VM도 있으며, GNU의 GCJ나 아파치 소프트웨어 재단(ASF: Apache Software Foundation)의 하모니(Harmony)와 같은 아직은 완전하지 않지만 지속적인 오픈 소스 자바 가상 머신도 존재한다.

subject_entity: 썬 마이크로시스템즈
object_entity: 오라클

relation: 단체:별칭 (org:alternate_names)
```

### B. 개발 장비 및 환경

| GPU | Tesla v100 (6인 1팀) |
| --- | --- |
| 개발 환경 | Ubuntu 18.04 |
| 협업 툴 | Github, Notion, Slack, Wandb |

| pandas | 1.1.5 | numpy | 1.19.2 |
| --- | --- | --- | --- |
| torch | 1.7.1 | scikit-learn | 0.24.2 |
| transformers | 4.10.0 | tokenizers | 0.10.3 |
| black | 23.3.0 | tqdm | 4.64.1 |
| omegaconf | 2.3.0 | wandb | 0.15.1 |

### C. 데이터셋

- Train 데이터 개수 : 32,470개
- Test 데이터 개수 : 7,765개
    - 정답 라벨의 경우 blind = 100으로 임의로 표현함
- 데이터 유형 (Source)
    - wikipedia
    - wikitree
    - policy_briefing
- dict_label_to_num.pkl: 문자 label과 숫자 label로 표현된 dictionary (총 30개 classes)
- dict_num_to_label.pkl: 숫자 label과 문자 label로 표현된 dictionary (총 30개 classes)

<img src="https://github.com/Jiwonii97/boostcampaitech5_klue-nlp-01/assets/79522982/dea7022d-448e-470a-8ae8-0704ad8d6093" alt="drawing" width="500"/>

# 2. 프로젝트  팀  구성  및  역할

- 김효연 : 데이터 분석, 단순 증강, val_dataset 예측 코드 작성
- 서유현 : 코드 리뷰어, 베이스 코드 작성 및 일반화 개선
- 손무현 : 코드 리뷰어, 데이터 분석 및 증강
- 이승진 : Loss 리서치, 편의 기능 제공, TAPT 연구
- 최규빈 : 데이터 튜닝, 모델 서치, 앙상블
- 황지원 : PM, Git 코드 버전 관리, 모델링 및 임베딩 작업 진행

# 3. 프로젝트  수행  절차  및  방법

### A. 팀 목표 설정

**(1주차)** 대회 간 팀 규칙을 설정하였고 Git Flow에 맞게 브랜치를 설정, PR(Pull-Request)에 대한 규칙을 정하였습니다. 팀 내에서 PM, 코드 리뷰어, 모델팀/데이터 팀 등 각각 역할을 분담하여 작업을 진행하였습니다. 전체적인 데이터 분포와 라벨을 확인하고 분석을 진행하였습니다. 또한 Focal Loss와 Special Entity Marker에 대한 탐색을 진행하였습니다.

---

**(2주차)** 이전 주차에 이어서 추가로 파라미터와 Loss에 대해 테스트를 진행하였습니다. 또한 데이터 부분에서 데이터 증강을 진행하였고 그와 함께 일부 학습 데이터에 대해 직접 데이터를 확인하고 분석하고 라벨링을 다시 하는 작업을 진행하였습니다.

---

**(3주차)** 마지막 주차인 만큼 다양한 테스트를 진행하기보다는 성능을 높일 수 있을 만한 세부 테스트를 진행하였습니다. TAPT와 Entity Marker, 프롬프트(Prompt)에 대해 코드 작성 후 테스트를 진행하였습니다. 그리고 Voting 방식을 채택해 soft/hard voting을 적용한 모델 앙상블 테스트를 마지막으로 진행하고 대회를 마무리하였습니다.

### B. 프로젝트 구성

```powershell
📦 level2_klue-nlp-01
├─ code
│  ├─ config    # 학습/테스트를 진행하는 config 파일
│  │  └─ config.yaml    # 기본 Task를 위한 config 파일
│  ├─ constant    # 코드 내에서 사용하는 상수
│  │  └─ CONFIG.py
│  ├─ custom
│  │	├─ custom_dataset.py
│  │	├─ custom_model.py
│  │	├─ custom_trainer.py
│  │  └─ entity_special_token.txt
│  ├─ log  # 모델 학습을 진행한 결과물
│  │  └─ # 
│  ├─ prediction  # Inference를 진행한 검증 결과물
│  │  └─ # 
│  ├─ utils
│  │  ├─ config.py    # config 전처리 작업
│  │  └─ log.py       # log 전처리 작업
│  ├─ dict_label_to_num.pkl
│  ├─ dict_num_to_label.pkl
│  ├─ load_data.py
│  ├─ train.py
│  ├─ inference.py
│  ├─ run.py
│  └─ requirements.txt
├─ dataset (.gitignore로 공유되지 않도록 설정)
│  ├─ train.csv
│  └─ test_data.csv
├─ .gitignore
└─ README.md
```

### C. 프로젝트 타임라인
<img src="https://github.com/Jiwonii97/boostcampaitech5_klue-nlp-01/assets/79522982/2466235a-b05f-4730-837b-aad2f989b688" alt="drawing" width="700"/>
