# Spatial-Bigdata-analysis - Using Rstudio
공간빅데이터분석 수업 최종 프로젝트 입니다.
주제: 통근시간 영향요인 분석: 수도권을 중심으로

docs파일에 있는 "통근시간 영향요인 분석_수도권을 중심으로.html"에 자세히 정리되어 있습니다.

---
title: "Spatial Bigdata Analysis (Final Project)_B913045_Park Jun Yeong"
author: "Park Jun Yeong"
date: "2023-12-12"
output:
  html_document:
    fig.retina: 2
    toc: true
    toc_float: 
          collapsed: true
    code_folding: hide
---

<style type="text/css">

body{ /* Normal  */s
      font-size: 14px;
  }   
td {  /* Table  */
  font-size: 10px;
}
h1.title {
  font-size: 32px;
  font-family: "serif", Times, serif;
  color: DarkBlue;
}
h1 { /* Header 1 */
  font-size: 22px;
  font-family: "serif", Times, serif;
  color: DarkBlue;
}
h2 { /* Header 2 */
    font-size: 16px;
    font-family: "serif", Times, serif;
    color: DarkBlue;
}
h3 { /* Header 3 */
  font-size: 13px;
  font-family: "serif", Times, serif;
  color: DarkBlue;
}
h4 { /* Header 4 */
  font-size: 12px;
  color: DarkBlue;
}
h5 { /* Header 5 */
  font-size: 10px;
  color: Black;
}
code.r{ /* Code block */
    font-size: 12px;
}
pre { /* Code block - determines code spacing between lines */
    font-size: 14px;    
}
</style>



<br>

# [주제] 통근시간 영향요인 분석: 수도권을 중심으로

"통근시간은 근로자의 직장 접근성을 나타내는 지표이다. Kim J. H. (2016)은 통근시간이 길어지는 만큼 업무관련 스트레스가 높아지며, 통근시간은 가족과의 활동시간에도 영향을 주기 때문에 삶의 질과 밀접한 관련이 있음을 밝혔다. 또한, 통근시간을 줄여 직주근접을 실현하는 것이 온실가스 배출 측면에서도 중요하다.

그런데, OECD 국가의 웰빙(well-being) 수준을 측정하기 위해 각국의 통근시간을 조사한 결과 우리나라는 평균 58분으로 OECD 국가내 압도적 1위를 차지했다.(OECD, 2016) 또한, KTDB의 자료에 의하면 2000년에서 2020년으로 갈 수록 통근시간이 15분미만은 지속적으로 감소하고, 30분 이상은 증가하는 추세를 보였다. 직장인은 도시의 성장과 주택 가격의 상승으로 인해 도시의 외곽에 거주할 가능성이 높아 통근시간이 불가피하게 증가하고 있는 실정이다.

통근시간을 줄이기 위해서는 다양한 영향요인을 고려해야 한다. 그 중 대표적으로 주거비용은 가장 크게 영향을 미칠 수 있다. 거주지의 주거비용이 높아질 수록 근로자들은 주거비용이 낮은 지역으로 이동하기 때문이다. 보편적으로 일자리가 많은 곳의 주거비용이 높기 때문에 통근시간은 증가하는 것을 알 수 있다. 이처럼 주거비용이 통근시간에 어떻게 영향을 미치는지와 어떤 요인이 통근시간에 영향을 미치는지 분석하는 것이 필요함을 알 수 있다.

특히, 소득계층에 따라 주택가격이 통근시간에 미치는 영향력은 다를 수 있다. 저소득층의 경우 주거비용이 부족해 일자리와 먼 곳에서 통근할 가능성이 높다. 고소득층은 저소득층에 비해 주거비용에 덜 민감할 가능성이 있지만, 추가적인 어메니티 요소의 영향을 받을 수 있다. 이에 대한 구체적인 분석이 필요할 것이다.

연구 대상은 출근 목적으로 자택이 출발지이고 직장이 도착지인 응답자만을 대상으로 진행하였다. 연구의 시간적 범위는 2021년, 공간적 범위는 도시교통정비촉진법에서 지정한 도시교통정비지역 및 교통권역 자료를 활용하여 서울특별시를 중심으로 교통과 밀접한 관계를 가지는 수도권 지역을 다음과 같이 선정하였다.
"

```{r}
incheon <- c('중구', '동구', '남구', '연수구', '남동구', '부평구', '계양구', '서구', '강화군')
gyeongido <- c('성남시 수정구', '성남시 중원구', '성남시 분당구', '의정부시', '안양시 만안구', '안양시 동안구', '부천시 원미구', '부천시 소사구', '부천시 오정구', '광명시', '고양시 덕양구', '고양시 일산동구', '고양시 일산서구', '과천시', '구리시', '남양주시', '하남시', '김포시', '양주시')

cat('도시교통정비지역은 서울특별시 전체이며,', '\n',  '교통권역으로 ',
    paste('경기도', gyeongido,',', sep = ' ' ),'\n', paste('인천광역시', incheon,',', sep = ' '), '이다.')

```
* 2022.06.22  수도권 통근자들의 “통근 스트레스” 관련 기사
- https://www.asiae.co.kr/article/2022061711005781949

<img src="C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/4학년/공간빅데이터분석/최종과제/그림1.jpg" style="width:60%;" />

* 2021.09.08  인구의 거주, 연령별 통근시간
- https://www.chosun.com/culture-life/living/2021/09/08/WRLDT54PMZGEZEF4NVMEPH5NEU/

<img src="C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/4학년/공간빅데이터분석/최종과제/그림2.jpg" style="width:60%;" />

* 2022.02.23  대한민국 출퇴근 시간
- https://www.newspost.kr/news/articleView.html?idxno=97011

<img src="C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/4학년/공간빅데이터분석/최종과제/그림3.jpg" style="width:60%;" />


<br>


# [질문] 분석 질문

본 연구에서는 2021년 개인통행실태조사를 활용하여 다음의 질문에 대하여 분석을 진행하고자 한다.

- 1. 통근시간에 영향을 미치는 요인은 무엇이 있고 통근시간과의 관계는 어떻게 되는가?
- 2. 소득계층별 평균 통근시간은 어떻게 다르며, 주거지와 직장지의 분포는 어떻게 다른가?
- 3. 거주지와 직장지의 주택가격에 따라 통근시간은 어떻게 달라지며 저소득층일 수록 주택가격이 통근시간에 미치는 영향력이 큰가?


<br>


# [데이터] 데이터 출처 

위의 분석 질문들에 답하기 위하여 다음의 데이터를 사용하였다. 

- 2021 개인통행실태조사: 
  - 조사목적: 개인통행실태조사는 국토교통부가 5년마다 정기적으로 실시해온 전국 여객통행조사의 일부이다. 전국 여객통행조사는 인구 및 사회, 국토 공간구조, 교통체계의
              변화로 인한 국민 통행 행태의 변화를 파악하기 위해 실시하는 조사로서 여객 기‧ 종점통행량 자료(O/D)1)자료를 구축하기 위한 목적으로 실시된다.
              또한,조사 가구의 가구원에 대해 평일 하루의 통행일지(Trip Diary)를 조사함으로써 지역 내 통행 행태를 파악하고 교통 SOC 투자의 기초자료인 여객기 종점 통행량 자료구축에 활용” 하기                   위한 목적으로 수행되는 조사이다.
              
  - 조사대상: 개인통행실태조사 : 2019년 인구주택총조사 전국 인구(5세 이상)의 0.23%(107천명)
  
  - 조사주기: 5년, 2016년 가구통행실태조사에서 개이통행실태조사로 이름 변경.
  
  - 조사방법: 가구방문설문조사, 면접조사, 영상촬영조사
  
  - 조사내용: 가구 및 가구원 현황조사
              - 가구원수, 주택종류, 가구월평균소득, 차량보유여부, 차량보유대수, 이륜차 및 기타차량 보유대수,가구주와의 관계, 출생년도, 성별, 운전면허보유여부, 교육기관재학 유무, 직업(주                       평균근무일수, 직장위치, 하루통행이 많은 직업 여부, 일평균 근무시간)
              개인별 통행특성
              - 통행일자, 조사당일 통행유무(미통행사유 포함), 출생년도, 성별
                출발․도착시간, 통행목적, 교통수단, 목적지 및 환승지, 최종목적지, 운전자포함 탑승인원
  - 출처: https://www.ktdb.go.kr/www/contents.do?key=202

- 주택 실거래가 데이터: 
  - 국토교통부에서는 정부3.0 및 공공데이터 개방, 실거래자료 요청 증가에 따라, 부동산 거래 가격 및 거래 동향을 보다 정확하고 신속히 파악할 수 있도록 부동산 거래신고제를 통해 수집된 실거래          자료를 공개하고 있다.
  
  - 매매 공개 대상: 매매 실거래 공개는 2006년 1월부터 부동산거래신고 및 주택거래신고를 한 주택(아파트, 연립/다세대, 단독/다가구), 오피스텔, 토지, 상업·업무용, 공장·창고 등 부동산 및 2007년 6월     29일 이후 체결된 아파트 분양/입주권을 대상으로 하고 있다.
    본 연구에서는 아파트, 연립주택, 단독, 오피스텔 매매 데이터만을 사용했다.
  
  -출처:  https://rtdown.molit.go.kr/


<br>


# [분석1] 통근시간 영향요인 분석

## [분석1.1] 개인통행실태조사 내에 존재하는 설명변수와 통근시간과의 관계가 어떻게 되는가?  {.tabset}

분석에 앞서 주거지와 직장이 도시교통정비지역 및 교통권역 지역에 속하는 곳을 선택한 후, 출근 목적으로 자택이 출발지이고 직장이 도착지인 데이터만 선정하는 과정을 거쳤다. 또한, 통근시간을 출발시간과 도착시간의 차이로 계산했으며, 180분이상의 극단치는 제거하였다.

개인통행실태조사 내에 존재하는 변수 중 선행연구에서 통근시간 영향요인 분석에서 주로 사용한 성별, 나이, 주요 이용 교통수단, 직업, 거주지역, 운전면허증 여부, 총가구원 수, 주택유형, 운전여부와 2019~2020년 코로나 상황을 반영하여 일주일 중 평균 재택근무일수를 통근시간과의 관계를 분석하고, 상관관계를 분석하여 어떤 요인이 통근시간에 큰 영향을 미치는지 알아보고자 한다.

```{r warning = FALSE, message = FALSE}
#기본 전처리
set.seed(123)
library(dplyr)    
library(tidyverse)
library(plotly)
df1 <- read.csv('C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/개인특성.csv', header = T, fileEncoding = "CP949", 
               encoding = "UTF-8")
df2 <- read.csv('C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/이동특성.csv', header = T, fileEncoding = "CP949", 
               encoding = "UTF-8")
df <- df1 %>% 
  left_join(df2, by= c('idx'))
# 출근 목적으로 자택이 출발지이고 직장이 도착지인 데이터 추출
df <- df[df$TP2 == 3,]
df <- df[df$sTP1 == 1 & df$TP1 == 2,]
# 주거지와 직장이 도시교통정비지역 및 교통권역 지역인 곳만 추출
df <- df[df$sTP1_1_6 == '서울특별시' | df$sTP1_1_6 == '서울' | df$sTP1_1_6 == '경기'| df$sTP1_1_6 == '경기도' | df$sTP1_1_6 == '인천' | df$sTP1_1_6 == '인천광역시', ]
df <- df[df$TP1_1_6 == '서울특별시' | df$TP1_1_6 == '서울' | df$TP1_1_6 == '경기'| df$TP1_1_6 == '경기도' | df$TP1_1_6 == '인천' | df$TP1_1_6 == '인천광역시', ]

df <- df[df$sTP1_1_6 == '서울' | df$sTP1_1_6 == '서울특별시' | df$sTP1_1_7 %in% incheon | df$sTP1_1_7 %in% gyeongido, ]
df <- df[df$TP1_1_6 == '서울' | df$TP1_1_6 == '서울특별시' | df$TP1_1_7 %in% incheon | df$TP1_1_7 %in% gyeongido, ]

df <- df %>% 
  mutate(sTP1_1_6 = case_when(
    sTP1_1_6 == '서울' ~ '서울특별시',
    sTP1_1_6 == '경기' ~ '경기도',
    sTP1_1_6 == '인천' ~ '인천광역시',
    TRUE ~ sTP1_1_6),
    TP1_1_6 = case_when(
      TP1_1_6 == '서울' ~ '서울특별시',
      TP1_1_6 == '경기' ~ '경기도',
      TP1_1_6 == '인천' ~ '인천광역시',
      TRUE ~ TP1_1_6
    ))

# Null값 제거
for (i in c('sTP1_1_6', 'TP1_1_6', 'sTP1_1_7', 'TP1_1_7')){
  df <- df[complete.cases(df[, i]), ]
}

#통근시간 계산
df <- df %>% 
  mutate(commute_time = (TP4_1 * 60 + TP4_2) - (TP3_1 * 60 + TP3_2)) %>% 
  filter(!(commute_time >= 180))

df_22 <- df %>% 
  rename(sex = SQ2,
         age = SQ3_1,
         total_household = Q1,
         car_license = R1,
         job = R3_2,
         list_home = Q2,
         yes_home_job = R4_1,
         major_trans = R4_3,
         drvie_yes = R1_1,
         sido_home = SQ1_6,
         start_bup_code = sTP1_1_4,
         destin_bup_code = TP1_1_4,
         출발_시도 = sTP1_1_6,
         도착_시도 = TP1_1_6,
         출발_시군구 = sTP1_1_7,
         도착_시군구 = TP1_1_7
         )
df_22 <- df_22[, c('sex', 'age', 'total_household','car_license','job', 'list_home',
                   'yes_home_job', 'major_trans', 'drvie_yes', 'sido_home', 'commute_time','start_bup_code', 'destin_bup_code', 
                   '출발_시도', '도착_시도', '출발_시군구', '도착_시군구')]
print(head(df_22),5)
```
### 전체 변수 결측치 확인

결측치 확인 결과 재택근무여부와 운전여부 변수의 결측치가 86%가 넘어가기 때문에, 2가지 변수는 제외하고 분석 진행

```{r warning = FALSE, message = FALSE}


colSums(is.na(df_22)) / nrow(df_22) * 100

```


<br>
<br>

### 성별 데이터 분석

데이터적으로 남자가 여자보다 많은 것을 확인할 수 있었으며, 남자가 여자보다 평균값이 약 5분 높았다.
상자그림을 봤을 때, 중앙값은 40분으로 마찬가지로 여자보다 높은 통근시간을 가졌다.
성별은 범주형 변수이기 때문에, Point Biserial Correlation을 이용하였다.
상관관계는 약 -0.10으로 약한 음의 상관관계를 가졌다.

```{r warning = FALSE, message = FALSE}
df_22$sex <- ifelse(df_22$sex == 1, '남자', '여자')
tb_sex <- table(df_22$sex)
print(tb_sex)

agg <- aggregate(df_22[,'commute_time'], by = list(sex = df_22$sex), mean);agg

ggplot(df_22, aes(x = sex, y = commute_time, fill = sex))+
  geom_boxplot(alpha = 0.7)+
  ggtitle('성별간 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "성별", y = "통근시간")+
  scale_fill_manual(values = c("남자" = "blue", "여자" = "red"))

df_22$sex <- ifelse(df_22$sex == '남자', 1, 2)
print(cor.test(df_22$commute_time, df_22$sex))

```


<br>
<br>

### 나이 데이터 분석

30, 40대의 데이터가 가장 많았으며, 이는 출근하는 데이터만 추출했기 때문으로 생각된다.
30대의 평균 통근시간이 가장 길었다.
데이터 분포는 원형의 데이터 분포를 가졌으며, 특별한 경향성을 보이지는 않았다.
상자그림을 봤을 때, 연령대별로 비슷한 정상 범위를 가졌다.
피어슨 상관관계 결과, -0.04로 매우 약한 음의 관계를 가졌다.

```{r warning = FALSE, message = FALSE}
df_22 <- df_22 %>% 
  mutate(age_group = case_when(age >= 10 & age < 20 ~ '10대',
                               age >= 10 & age < 30 ~ '20대',
                               age >= 30 & age < 40 ~ '30대',
                               age >= 40 & age < 50 ~ '40대',
                               age >= 50 & age < 60 ~ '50대',
                               age >= 60 & age < 70 ~ '60대',
                               TRUE ~ '70대 이상'))
tb_age <- table(df_22$age_group)
print(tb_age)

agg_age <- aggregate(df_22[,'commute_time'], by = list(age_group = df_22$age_group), mean)
agg_age <- agg_age[order(agg_age$x, decreasing = T),];agg_age

ggplot(df_22, aes(x = age, y = commute_time, col = age_group))+
  geom_point()+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('나이와 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "나이", y = "통근시간")

ggplot(df_22, aes(x = age_group, y = commute_time, fill = age_group))+
  geom_boxplot(alpha = 0.7)+
  ggtitle('나이대별 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "나이대", y = "통근시간")+
  scale_fill_manual(values = c(
    '10대' = 'red',
    '20대' = 'orange',
    '30대' = 'yellow',
    '40대' = 'green',
    '50대' = 'blue',
    '60대' = 'purple',
    '70대 이상' = 'brown'))

cat('상관계수 : ', cor(df_22$age, df_22$commute_time))
```


<br>
<br>

### 가구원 수 데이터 분석

1명부터 4명까지의 가구원 수를 가지는 경우가 가장 많았으며, aggregate를 했을 때, 가구원 수가 극단적으로 많을 수록 평균 통근시간이 대체적으로 높은 경향을 보였다.
산점도 상으로, 경향성은 보이지 않았으며, 상관관계는 약 0.05로 약한 양의 상관관계를 가짐을 알 수 있었다.

```{r warning = FALSE, message = FALSE}
tb_hh <- table(df_22$total_household)
print(tb_hh)

ggplot(df_22, aes(total_household))+
  geom_histogram(bins = 30, fill=rgb(1,0,0,0.5))+
  ggtitle('가구원 수 분포')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "가구원 수", y = "count")
  

agg_hh <- aggregate(df_22[,'commute_time'], by = list(total_household = df_22$total_household), mean)
agg_hh <- agg_hh[order(agg_hh$x, decreasing = T),];agg_hh

ggplot(df_22, aes(x = total_household, y = commute_time))+
  geom_point(col = 'blue', alpha=0.7)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('가구원 수와 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "가구원 수", y = "통근시간")

cat('상관계수 : ', cor(df_22$total_household, df_22$commute_time))
```


<br>
<br>

### 운전면허증 보유 여부 데이터 분석

운전면허증을 보유한 경우 자동차를 이용해서 출퇴근하여 보다 낮은 통근시간을 예상했다. 실제로 운전면허증을 보유한 경우 평균 통근시간이 약 1분더 높았다. 상자그림을 봤을 때는 큰 차이는 나타나지 않았다.
상관계수도 약 0.01로 매우 약한 양의 상관관계를 가졌다.

```{r warning = FALSE, message = FALSE}
df_22$car_license <- ifelse(df_22$car_license == 1, '있음', '없음')
tb_cl <- table(df_22$car_license)
print(tb_cl)

agg_cl <- aggregate(df_22[,'commute_time'], by = list(car_license = df_22$car_license), mean)
agg_cl <- agg_cl[order(agg_cl$x, decreasing = T),];agg_cl

ggplot(df_22[!is.na(df_22$car_license), ], aes(x = car_license, y = commute_time, fill = car_license))+
  geom_boxplot(alpha = 0.7)+
  ggtitle('운전면허증 여부와 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "운전면허증 보유 여부", y = "통근시간")+
  scale_fill_manual(values = c("없음" = "hotpink2", "있음" = "cyan3"))

df_22$car_license <- ifelse(df_22$car_license == '있음', 1, 2)
print(cor.test(df_22$car_license, df_22$commute_time))
```


<br>
<br>

### 응답자 직업 데이터 분석

1:자영업, 2: 판매 및 서비스 직, 3: 기능/숙련직, 4: 일반 작업직, 5: 기술직, 6: 사무직, 7: 관리직, 8: 전문직, 9:자유직, 10:단순노무종사자, 11: 농림/어업종사자, 97: 기타

사무직이 가장 데이터가 많았으며, 기술직, 판매 및 서비스직 순서로 많았다.
평균통근시간은 관리직, 사무직, 기술직 순으로 높은 것을 확인할 수 있다.
상자그림을 봤을 때, 농림/어업 종사자를 제외한 케이스에서는 비슷한 모습을 보였다.

자세한 분석을 위해 전문직, 사무직, 관리직은 1로, 자영업과 판매 및 서비스 직은 2로, 기술직, 기능숙련직을 포함한 기타 직업은 3으로 묶어서 코딩하였다.
전문직, 사무직, 관리직의 통근시간이 다른 직업보다 더 높은 것을 확인할 수 있다.

상관관계는 약 -0.04로 통근시간과 매우 약한 음의 상관관계를 가진다.

```{r warning = FALSE, message = FALSE}
tb_job <- table(df_22$job)
print(sort(tb_job, decreasing = T))

agg_job <- aggregate(df_22[,'commute_time'], by = list(job = df_22$job), mean)
agg_job <- agg_job[order(agg_job$x, decreasing = T),];agg_job

df_22$job <- as.character(df_22$job)
ggplot(df_22, aes(x = job, y = commute_time, fill = job))+
  geom_boxplot()+
  ggtitle('직업과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "직업", y = "통근시간")+
  scale_fill_brewer(palette = "Set3")

df_22 <- df_22 %>% 
  mutate(job_group = case_when(job == 8 | job == 6 | job == 7 ~ '1',
                               job == 1 | job == 2 ~ '2',
                               TRUE ~ '3'))

library(viridis)
df_22$job_group <- as.character(df_22$job_group)
ggplot(df_22, aes(x = job_group, y = commute_time, fill = job_group))+
  geom_boxplot(alpha = 0.7)+
  ggtitle('직업과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "직업", y = "통근시간")+
  scale_fill_viridis_d(option = "Royal2")

df_22$job <- as.numeric(df_22$job)
print(cor.test(df_22$job, df_22$commute_time))

```


<br>
<br>

### 주요 이용 교통수단 데이터 분석

이용 교통수단은 자동차를 1, 버스를 2, 지하철을 3, 보행/자전거/전동킥보드를 4, 기타를 5로 코딩하고 분석을 진행하였다.

지하철을 주요 이용 교통수단을 이용하는 응답자들의 경우 평균 통근시간이 약 59분으로 압도적으로 높았으며, 다음으로 버스가 높았다. 보행/자전거/전동킥보드 이용자들은 일자리가 가깝기 때문에 평균 통근시간도 가장 짧은 것을 확인할 수 있다. 상자그림에서도 같은 결과가 나타난다.

즉, 대중교통을 이용할 경우 통근시간이 높은 것을 확인할 수 있었다.
상관계수는 0.03으로 매우 약한 양의 상관관계를 가졌다.


```{r warning = FALSE, message = FALSE}
df_22 <- df_22 %>% 
  mutate(trans = case_when(major_trans == 2 | major_trans == 10 ~ '1',
                           major_trans == 3 | major_trans == 4 | major_trans == 5 ~ '2',
                           major_trans == 7 | major_trans == 8 | major_trans == 9 ~ '3',
                           major_trans == 1 | major_trans == 12 | major_trans == 14 ~ '4',
                           TRUE ~ '5'))

agg_trans <- aggregate(df_22[,'commute_time'], by = list(trans = df_22$trans), mean)
agg_trans <- agg_trans[order(agg_trans$x, decreasing = T),];agg_trans

ggplot(df_22, aes(x = trans, y = commute_time, fill = trans))+
  geom_boxplot()+
  ggtitle('주요 이용 교통수단과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "교통수단", y = "통근시간")+
  scale_fill_brewer(palette = "Set2")

df_22$trans <- as.numeric(df_22$trans)
print(cor.test(df_22$trans, df_22$commute_time))

```

<br>
<br>

### 거주 지역 데이터 분석

개인적으로 거주 지역이 서울특별시일 경우에 거주지역이 경기도나 인천광역시인 응답자들보다 통근시간이 적을 것이라고 생각하고, 두 집단 간 유의한 차이가 있을 것으로 생각하기에 분석하고자 한다. 거주지가 서울일 경우 1로, 나머지는 0으로 코딩하였다.

실제로 서울거주자가 아닌 경우 통근시간이 약 1분 더 높은 것으로 나타났다. 상자그림에서도 마찬가지였다.
상관계수는 -0.02로 매우 약한 음의 상관관계를 가졌다.

```{r warning = FALSE, message = FALSE}
df_22 <- df_22 %>% 
  mutate(seoul_yes = ifelse(sido_home == '서울특별시' | sido_home == '서울', '1', '0'))

df_22_summarise <- df_22 %>% 
  group_by(seoul_yes) %>% 
  summarise(mean_commute_time = mean(commute_time))
print(df_22)

ggplot(df_22, aes(x = seoul_yes, y = commute_time, fill = seoul_yes))+
  geom_boxplot(alpha = 0.7)+
  ggtitle('거주 지역과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "서울 거주여부", y = "통근시간")+
  scale_fill_brewer(palette = "Set1")

df_22$seoul_yes <- as.numeric(df_22$seoul_yes)
print(cor.test(df_22$seoul_yes, df_22$commute_time))
```

<br>


# [분석2] 소득계층별 통근시간 분석

## [분석2.1] 소득계층별로 평균 통근시간은 어떻게 다르게 나타나는가? {.tabset}

### 소득계층별 데이터 개수

개인통행실태조사에서는 가구별 월 소득이 100만원 미만, 100만원 ~ 300만원 미만, 300만원 ~ 500만원 미만, 500만원 ~ 1,000만원 미만, 1,000만원 이상 ~ 1,500만원 미만, 1,500만원 이상으로 구분되어 있다. 
본 연구에서는 분석의 편의성을 위해 300만원 미만을 저소득층, 300만원~500만원을 중소득층, 500만원 이상을 고소득층으로 구분하였다.

분석결과 고소득층의 데이터가 가장 많으며, 중소득층, 저소득층 순서로 데이터가 많은 것을 확인할 수 있었다.

```{r warning = FALSE, message = FALSE}
df <- df %>% 
  mutate(income_status = case_when(Q3 == 1 | Q3 == 2 ~ '저소득층',
                                   Q3 == 3 ~ '중소득층',
                                   Q3 == 4 | Q3 == 5 | Q3 == 6 ~ '고소득층',
                                   TRUE ~ '자료없음'))
table(df$income_status)
```


```{r warning = FALSE, message = FALSE}
#자료없음 데이터 제거
df <- df[df$income_status != '자료없음', ]

#소득계층 열 추가하기
df_22 <- df %>% 
  rename(sex = SQ2,
         age = SQ3_1,
         total_household = Q1,
         car_license = R1,
         job = R3_2,
         list_home = Q2,
         yes_home_job = R4_1,
         major_trans = R4_3,
         drvie_yes = R1_1,
         sido_home = SQ1_6,
         start_bup_code = sTP1_1_4,
         destin_bup_code = TP1_1_4,
         출발_시도 = sTP1_1_6,
         도착_시도 = TP1_1_6,
         출발_시군구 = sTP1_1_7,
         도착_시군구 = TP1_1_7
         )
df_22 <- df_22[, c('sex', 'age', 'total_household','car_license','job', 'list_home',
                   'yes_home_job', 'major_trans', 'drvie_yes', 'sido_home', 'commute_time','start_bup_code', 'destin_bup_code', 
                   '출발_시도', '도착_시도', '출발_시군구', '도착_시군구', 'income_status')]

#데이터 개수 세기
income_counts <- df %>%
  count(income_status)
income_counts$income_status <- factor(income_counts$income_status, levels=c('저소득층','중소득층','고소득층'))

library(RColorBrewer)

ggplot(income_counts, aes(x = income_status, y = n, fill = income_status))+
  geom_bar(stat = 'identity', alpha = 0.8)+
  scale_fill_brewer(palette = "YlOrRd")+
  ggtitle('소득계층별 데이터 개수')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue')) +
  labs(x = "소득 계층", y = "데이터 개수")

#비율로 확인
income_counts <- income_counts %>% 
  mutate(pct = n / sum(n) * 100)

ggplot(income_counts, aes(x='', y=pct, fill=income_status))+
  geom_bar(stat='identity', alpha = 0.8)+
  theme_void()+
  coord_polar('y', start=0)+
  geom_text(aes(label=paste0(round(pct,1), '%')),
            position=position_stack(vjust=0.5))+
  scale_fill_brewer(palette = "YlOrRd")+
  ggtitle('소득계층별 데이터 비율')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))
```

<br>
<br>


### 소득계층별 통근시간 비교 - 막대그래프

평균값으로 계산했을 때, 고소득층이 약 45분, 중소득층이 43분, 저소득층이 40분으로 고소득층이 가장 긴 것을 확인할 수 있었다.
중앙값으로 계산했을 때는 고소득층이 40분, 중소득층이 40분, 저소득층이 35분으로 고소득층과 중소득층이 통근시간이 가장 긴 것을 확인할 수 있다.

```{r warning = FALSE, message = FALSE}

df$income_status <- factor(df$income_status, levels=c('저소득층','중소득층','고소득층'))
df_mean <- aggregate(df$commute_time, by = list(income = df$income_status), FUN = mean)
names(df_mean) <- c('income', 'commute_time_mean')

# 소득계층별 평균 통근시간 막대그래프
ggplot(df_mean , aes(x = income, y = commute_time_mean, fill = income))+
  geom_bar(stat = 'identity', alpha = 0.8)+
  ggtitle('소득계층별 통근시간 평균값 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  scale_fill_brewer(palette = "YlOrRd")+
  labs(x = "소득계층", y = "통근시간 평균")+
  coord_flip()

print(df_mean)

#소득계층별 중앙값 통근시간 막대그래프
df_median <- aggregate(df$commute_time, by = list(income = df$income_status), FUN = median)
names(df_median) <- c('income', 'commute_time_median')
ggplot(df_median , aes(x = income, y = commute_time_median, fill = income))+
  geom_bar(stat = 'identity', alpha = 0.8)+
  ggtitle('소득계층별 통근시간 중앙값 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  scale_fill_brewer(palette = "YlOrRd")+
  labs(x = "소득계층", y = "통근시간 중앙")+
  coord_flip()

print(df_median)

```

<br>
<br>

### 소득계층별 통근시간 비교 - 상자그림

저소득층이 가장 통근시간이 짧고, 중소득층과 고소득층의 중앙값은 똑같으나 중소득층의 정상범위가 더 큰 것을 확인할 수 있다.
세가지 계층간의 통근시간 분포 차이를 t-test한 결과 p-value가 0.05보다 모두 작게 나왔다.
세가지 계층간의 통근시간 차이가 통계적으로 유의하다는 것을 알 수 있다.

```{r warning = FALSE, message = FALSE}

library(ggsignif)
# 상자그림
ggplot(df, aes(x = income_status, y = commute_time, fill = income_status))+
  geom_boxplot(alpha = 0.8)+
  ggtitle('소득계층별 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  scale_fill_brewer(palette = "YlOrRd")+
  labs(x = "소득계층", y = "통근시간")+
  geom_signif(comparisons = list(c("저소득층","중소득층"),c("중소득층","고소득층"),c("저소득층","고소득층")),
              map_signif_level = F, step_increase = 0.1)

```


<br>

## [분석2.2] 소득계층별 주거지와 직장지 분포 {.tabset}

소득계층별로 소득계층별 주거지와 직장비 분포에 대해 분석했다. 법정동코드의 문제로 맵핑은 시군구 단위로 진행했다. 

### 저소득층

저소득층의 경우 주거지 개수의 경우 서울특별시 관악구(150개), 은평구(105개), 강서구(102개) 순으로 많았으며, 경기도 과천시, 인천광역시 동구, 강화군 순으로 적었다.
직장지 개수의 경우 강남구(223개), 중구(174개), 서초구(133개), 영등포구(123개) 순으로 많았으며, 주로 서울 3대 업무지구에 많은 것을 확인할 수 있다. 또한, 거주지 개수가 가장 적었던 
경기도 과천시, 인천광역시 동구, 강화군 순으로 똑같이 직장지 개수가 적었다. 주거지와 직장지가 서로 상이한 모습을 보였다.

```{r warning = FALSE, message = FALSE}
library(sf)

seoul_map <-  st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/서울/LARD_ADM_SECT_SGG_11.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
gyeongi_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/경기/LARD_ADM_SECT_SGG_41.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
incheon_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/인천/LARD_ADM_SECT_SGG_28.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)

gyeongi_map <- gyeongi_map %>%
  filter(SGG_NM == '성남시 수정구' | SGG_NM == '성남시 중원구' | SGG_NM == '성남시 분당구' | SGG_NM == '의정부시' | SGG_NM == '안양시만안구' | SGG_NM == '안양시동안구' | SGG_NM == '부천시' |
         SGG_NM == '광명시' | SGG_NM == '고양시덕양구' | SGG_NM == '고양시일산동구' | SGG_NM == '고양시일산서구' | SGG_NM == '과천시' | SGG_NM == '성남시 중원구' | SGG_NM == '구리시' | 
         SGG_NM == '남양주시' | SGG_NM == '하남시' | SGG_NM == '김포시' | SGG_NM == '양주시')

incheon_map <- incheon_map %>%
  filter(SGG_NM == '중구' | SGG_NM == '동구' | SGG_NM == '남구' | SGG_NM == '연수구' | SGG_NM == '남동구' | SGG_NM == '부평구' | SGG_NM == '계양구'|
         SGG_NM == '서구' | SGG_NM == '강화군') %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '중구' ~ '인천중구',
    TRUE ~ SGG_NM
  ))

map_list <- list(gyeongi_map, seoul_map, incheon_map)

# 모든 데이터를 하나로 합치기
combined_map <- do.call(rbind, map_list) %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '고양시일산서구' ~ '고양시 일산서구',
    SGG_NM == '고양시일산동구' ~ '고양시 일산동구',
    SGG_NM == '고양시덕양구' ~ '고양시 덕양구',
    SGG_NM == '안양시동안구' ~ '안양시 동안구',
    SGG_NM == '안양시만안구' ~ '안양시 만안구',
    TRUE ~ SGG_NM
  ))

##주거지
df_22_merge_startmean <- df_22 %>%
  filter(income_status == '저소득층') %>% 
  group_by(출발_시도, 출발_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(출발_시군구 = case_when(
    출발_시군구 == '중구' & 출발_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 출발_시군구
  ))

merged_map <- merge(combined_map, df_22_merge_startmean, by.x = "SGG_NM", by.y = "출발_시군구", all.x = TRUE)

merged_map_sf <- st_as_sf(merged_map)
#상위 3개만 레이블링
centroid <- st_centroid(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf$interval <- cut(merged_map_sf$n, breaks = 5)

# 주거지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "주거지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 저소득층 주거지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid), aes(label = SGG_NM, x = st_coordinates(centroid)[, "X"], y = st_coordinates(centroid)[, "Y"]),
            size = 2.3, color = "black")


print(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],5))
print(head(merged_map_sf[order(merged_map_sf$n),],5))

##직장지
df_22_merge_destmean <- df_22 %>%
  filter(income_status == '저소득층') %>% 
  group_by(도착_시도, 도착_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(도착_시군구 = case_when(
    도착_시군구 == '중구' & 도착_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 도착_시군구))

merged_map2 <- merge(combined_map, df_22_merge_destmean, by.x = "SGG_NM", by.y = "도착_시군구", all.x = TRUE)
merged_map_sf2 <- st_as_sf(merged_map2)

#상위 3개만 레이블링
centroid2 <- st_centroid(head(merged_map_sf2[order(merged_map_sf2$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf2$interval <- cut(merged_map_sf2$n, breaks = 5)

# 직장지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf2, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "직장지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 저소득층 직장지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid2), aes(label = SGG_NM, x = st_coordinates(centroid2)[, "X"], y = st_coordinates(centroid2)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map2[order(merged_map2$n, decreasing = T),],5))
print(head(merged_map2[order(merged_map2$n),],5))
```

<br>
<br>

### 중소득층

중소득층의 경우 주거지 개수의 경우 서울특별시 강서구(187개), 남양주시(172개), 송파구(172개) 순으로 많았으며, 경기도 과천시, 인천광역시 강화군, 동구 순으로 적었다.
직장지 개수의 경우 강남구(797개), 중구(561개), 영등포구(487개), 서초구(440개) 순으로 많았으며, 저소득층과 마찬가지로 주로 서울 3대 업무지구에 많은 것을 확인할 수 있다. 또한, 주거지 개수가 가장 적었던 순서와 비슷하게 인천광역시 강화군, 동구, 경기도 과천시 순으로 직장지 개수가 적었다. 주거지와 직장지가 서로 상이한 모습을 보였다.

```{r warning = FALSE, message = FALSE}
seoul_map <-  st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/서울/LARD_ADM_SECT_SGG_11.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
gyeongi_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/경기/LARD_ADM_SECT_SGG_41.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
incheon_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/인천/LARD_ADM_SECT_SGG_28.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)

gyeongi_map <- gyeongi_map %>%
  filter(SGG_NM == '성남시 수정구' | SGG_NM == '성남시 중원구' | SGG_NM == '성남시 분당구' | SGG_NM == '의정부시' | SGG_NM == '안양시만안구' | SGG_NM == '안양시동안구' | SGG_NM == '부천시' |
         SGG_NM == '광명시' | SGG_NM == '고양시덕양구' | SGG_NM == '고양시일산동구' | SGG_NM == '고양시일산서구' | SGG_NM == '과천시' | SGG_NM == '성남시 중원구' | SGG_NM == '구리시' | 
         SGG_NM == '남양주시' | SGG_NM == '하남시' | SGG_NM == '김포시' | SGG_NM == '양주시')

incheon_map <- incheon_map %>%
  filter(SGG_NM == '중구' | SGG_NM == '동구' | SGG_NM == '남구' | SGG_NM == '연수구' | SGG_NM == '남동구' | SGG_NM == '부평구' | SGG_NM == '계양구'|
         SGG_NM == '서구' | SGG_NM == '강화군') %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '중구' ~ '인천중구',
    TRUE ~ SGG_NM
  ))

map_list <- list(gyeongi_map, seoul_map, incheon_map)

# 모든 데이터를 하나로 합치기
combined_map <- do.call(rbind, map_list) %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '고양시일산서구' ~ '고양시 일산서구',
    SGG_NM == '고양시일산동구' ~ '고양시 일산동구',
    SGG_NM == '고양시덕양구' ~ '고양시 덕양구',
    SGG_NM == '안양시동안구' ~ '안양시 동안구',
    SGG_NM == '안양시만안구' ~ '안양시 만안구',
    TRUE ~ SGG_NM
  ))

##주거지
df_22_merge_startmean <- df_22 %>%
  filter(income_status == '중소득층') %>% 
  group_by(출발_시도, 출발_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(출발_시군구 = case_when(
    출발_시군구 == '중구' & 출발_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 출발_시군구
  ))

merged_map <- merge(combined_map, df_22_merge_startmean, by.x = "SGG_NM", by.y = "출발_시군구", all.x = TRUE)
merged_map_sf <- st_as_sf(merged_map)
#상위 3개만 레이블링
centroid <- st_centroid(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf$interval <- cut(merged_map_sf$n, breaks = 5)

# 주거지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "주거지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 중소득층 주거지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid), aes(label = SGG_NM, x = st_coordinates(centroid)[, "X"], y = st_coordinates(centroid)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],5))
print(head(merged_map_sf[order(merged_map_sf$n),],5))

##직장지
df_22_merge_destmean <- df_22 %>%
  filter(income_status == '중소득층') %>% 
  group_by(도착_시도, 도착_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(도착_시군구 = case_when(
    도착_시군구 == '중구' & 도착_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 도착_시군구))

merged_map2 <- merge(combined_map, df_22_merge_destmean, by.x = "SGG_NM", by.y = "도착_시군구", all.x = TRUE)
merged_map_sf2 <- st_as_sf(merged_map2)

#상위 3개만 레이블링
centroid2 <- st_centroid(head(merged_map_sf2[order(merged_map_sf2$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf2$interval <- cut(merged_map_sf2$n, breaks = 5)

# 직장지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf2, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "직장지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 중소득층 직장지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid2), aes(label = SGG_NM, x = st_coordinates(centroid2)[, "X"], y = st_coordinates(centroid2)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map_sf2[order(merged_map_sf2$n, decreasing = T),],5))
print(head(merged_map_sf2[order(merged_map_sf2$n),],5))
```


<br>
<br>

### 고소득층

고소득층의 경우 주거지 개수의 경우 서울특별시 송파구(360개), 강남구(296개), 강서구(257개) 순으로 많았으며, 인천광역시 강화군, 동구, 경기도 양주시 순으로 적었다. 
직장지 개수의 경우 강남구(797개), 중구(561개), 영등포구(487개), 서초구(440개) 순으로 많았으며, 저소득층과 마찬가지로 주로 서울 3대 업무지구에 많은 것을 확인할 수 있다. 또한, 거주지 개수가 가장 적었던 순서와 비슷하게 인천광역시 강화군, 경기도 과천시, 인천광역시 동구 순으로 직장지 개수가 적었다.
고소득층은 저,중소득층과 달리 직장이 많은 강남구, 송파구에 많이 거주하는 것으로 나타났다. 거주지와 직장지가 일치하다보니 고소득층의 통근시간이 가장 적을 것이라고 생각될 수 있으나, 위쪽에서 소득계층별 평균 통근시간을 비교했을 때 고소득층이 가장 통근시간이 높았다. 이러한 이유에 대한 추가적인 분석이 필요할 것으로 생각된다.

```{r warning = FALSE, message = FALSE}
seoul_map <-  st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/서울/LARD_ADM_SECT_SGG_11.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
gyeongi_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/경기/LARD_ADM_SECT_SGG_41.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
incheon_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/인천/LARD_ADM_SECT_SGG_28.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)

gyeongi_map <- gyeongi_map %>%
  filter(SGG_NM == '성남시 수정구' | SGG_NM == '성남시 중원구' | SGG_NM == '성남시 분당구' | SGG_NM == '의정부시' | SGG_NM == '안양시만안구' | SGG_NM == '안양시동안구' | SGG_NM == '부천시' |
         SGG_NM == '광명시' | SGG_NM == '고양시덕양구' | SGG_NM == '고양시일산동구' | SGG_NM == '고양시일산서구' | SGG_NM == '과천시' | SGG_NM == '성남시 중원구' | SGG_NM == '구리시' | 
         SGG_NM == '남양주시' | SGG_NM == '하남시' | SGG_NM == '김포시' | SGG_NM == '양주시')

incheon_map <- incheon_map %>%
  filter(SGG_NM == '중구' | SGG_NM == '동구' | SGG_NM == '남구' | SGG_NM == '연수구' | SGG_NM == '남동구' | SGG_NM == '부평구' | SGG_NM == '계양구'|
         SGG_NM == '서구' | SGG_NM == '강화군') %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '중구' ~ '인천중구',
    TRUE ~ SGG_NM
  ))

map_list <- list(gyeongi_map, seoul_map, incheon_map)

# 모든 데이터를 하나로 합치기
combined_map <- do.call(rbind, map_list) %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '고양시일산서구' ~ '고양시 일산서구',
    SGG_NM == '고양시일산동구' ~ '고양시 일산동구',
    SGG_NM == '고양시덕양구' ~ '고양시 덕양구',
    SGG_NM == '안양시동안구' ~ '안양시 동안구',
    SGG_NM == '안양시만안구' ~ '안양시 만안구',
    TRUE ~ SGG_NM
  ))

##주거지
df_22_merge_startmean <- df_22 %>%
  filter(income_status == '고소득층') %>% 
  group_by(출발_시도, 출발_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(출발_시군구 = case_when(
    출발_시군구 == '중구' & 출발_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 출발_시군구
  ))

merged_map <- merge(combined_map, df_22_merge_startmean, by.x = "SGG_NM", by.y = "출발_시군구", all.x = TRUE)
merged_map_sf <- st_as_sf(merged_map)
#상위 3개만 레이블링
centroid <- st_centroid(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf$interval <- cut(merged_map_sf$n, breaks = 5)

# 주거지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "주거지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 고소득층 주거지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid), aes(label = SGG_NM, x = st_coordinates(centroid)[, "X"], y = st_coordinates(centroid)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map_sf[order(merged_map_sf$n, decreasing = T),],5))
print(head(merged_map_sf[order(merged_map_sf$n),],5))

##직장지
df_22_merge_destmean <- df_22 %>%
  filter(income_status == '고소득층') %>% 
  group_by(도착_시도, 도착_시군구) %>% 
  summarise(n = n()) %>% 
  mutate(도착_시군구 = case_when(
    도착_시군구 == '중구' & 도착_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 도착_시군구))

merged_map2 <- merge(combined_map, df_22_merge_destmean, by.x = "SGG_NM", by.y = "도착_시군구", all.x = TRUE)
merged_map_sf2 <- st_as_sf(merged_map2)

#상위 3개만 레이블링
centroid2 <- st_centroid(head(merged_map_sf2[order(merged_map_sf2$n, decreasing = T),],3))

# natural break 나누기
merged_map_sf2$interval <- cut(merged_map_sf2$n, breaks = 5)

# 직장지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf2, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "직장지 수", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 고소득층 직장지 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid2), aes(label = SGG_NM, x = st_coordinates(centroid2)[, "X"], y = st_coordinates(centroid2)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map_sf2[order(merged_map_sf2$n, decreasing = T),],5))
print(head(merged_map_sf2[order(merged_map_sf2$n),],5))
```


<br>

# [분석3] 주택가격과 통근시간 분석 

주택가격 데이터의 경우 법정동 코드와 매칭시키는 전처리 작업을 먼저 수행했다. 주택가격은 면적당 평균 주택가격을 사용했다.
이후 시군구명, 법정동코드를 이용해서 2021 개인통행실태조사와 2021년 주택가격 데이터를 merge시켜주는 작업을 진행했다.
주택가격 데이터는 거주지와 직장지 2개의 변수로 각각 데이터를 만들었다.

```{r warning = FALSE, message = FALSE}
library(readxl)

#seoul
apart <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/아파트(매매)_실거래가_20231214201701.xlsx', skip = 16)
df_yunlib <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/연립다세대(매매)_실거래가_20231214201729.xlsx', skip = 16)
df_dandok <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/단독다가구(매매)_실거래가_20231214202004.xlsx', skip = 16)
df_offi <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/오피스텔(매매)_실거래가_20231214.xlsx', skip = 16)

#필요한 열 추출
apart <- apart[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_yunlib <- df_yunlib[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_dandok <- df_dandok[,c('시군구', '거래금액(만원)', '대지면적(㎡)')] %>% 
  rename('전용면적(㎡)' = '대지면적(㎡)')
df_offi <- df_offi[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]

total_seoul_price <- bind_rows(apart, df_yunlib, df_dandok, df_offi)

#공란 기준으로 열 나누고 정리
total_seoul_price <- total_seoul_price %>% 
  separate('시군구', into = c('시', '구', '동'), sep = ' ') %>%
  separate('거래금액(만원)', into = c('blank', 'price'), sep = '      ') %>%
  select(-price) %>%
  rename(price = blank,
         시군구명 = 구,
         동리명 = 동)

total_seoul_price[] <-  lapply(total_seoul_price, function(x) gsub(",", "", x))

total_seoul_price$price <- as.numeric(total_seoul_price$price)
total_seoul_price$'전용면적(㎡)' <- as.numeric(total_seoul_price$'전용면적(㎡)')

total_seoul_price <- total_seoul_price %>%
  mutate(price_per_size = total_seoul_price$price / total_seoul_price$'전용면적(㎡)') %>% 
  group_by(시군구명, 동리명) %>% 
  summarise(price_per_size = mean(price_per_size))

#법정동 코드 파일
code <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/행정동코드_20210726(말소코드포함).xlsx')
code <- code[!complete.cases(code$말소일자), ] %>% 
  select(시군구명,법정동코드, 동리명) %>% 
  distinct()

#법정동 코드와 merge
merge_price_seoul <- total_seoul_price %>% 
  left_join(code, by = c('시군구명','동리명'))

#경기도, 위와 동일한 방식으로 진행
apart_g <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/아파트(매매)_실거래가_20231215120929.xlsx', skip = 16)
df_yunlib_g <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/연립다세대(매매)_실거래가_20231215121108.xlsx', skip = 16)
df_dandok_g <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/단독다가구(매매)_실거래가_20231215121141.xlsx', skip = 16)
df_offi_g <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/오피스텔(매매)_실거래가_20231215.xlsx', skip = 16)

apart_g <- apart_g[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_yunlib_g <- df_yunlib_g[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_dandok_g <- df_dandok_g[,c('시군구', '거래금액(만원)', '대지면적(㎡)')] %>% 
  rename('전용면적(㎡)' = '대지면적(㎡)')
df_offi_g <- df_offi_g[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]

total_g_price <- bind_rows(apart_g, df_yunlib_g, df_dandok_g, df_offi_g)

#주택데이터에서는 '시'글씨가 빠져있어서 추가
total_g_price <- total_g_price %>%
  separate('시군구', into = c('시', '구', '동'), sep = ' ') %>%
  separate('거래금액(만원)', into = c('blank', 'price'), sep = '      ') %>%
  select(-price) %>%
  rename(price = blank,
         시군구명 = 구,
         동리명 = 동) %>% 
  mutate(시군구명 = case_when(
    시군구명 == '성남수정구' ~ '성남시 수정구',
    시군구명 == '성남중원구' ~ '성남시 중원구',
    시군구명 == '성남분당구' ~ '성남시 분당구',
    시군구명 == '안양만안구' ~ '안양시 만안구',
    시군구명 == '안양동안구' ~ '안양시 동안구',
    시군구명 == '고양덕양구' ~ '고양시 덕양구',
    시군구명 == '고양일산동구' ~ '고양시 일산동구',
    시군구명 == '고양일산서구' ~ '고양시 일산서구',
    TRUE ~ 시군구명
  )) %>%
  filter(시군구명 %in% c('성남시 수정구', '성남시 중원구', '성남시 분당구', '의정부시', '안양시 만안구', '안양시 동안구', '부천시', '광명시', '고양시 덕양구', '고양시 일산동구', '고양시 일산서구',
                         '과천시', '구리시', '남양주시', '하남시', '김포시', '양주시'))

total_g_price[] <-  lapply(total_g_price, function(x) gsub(",", "", x))

total_g_price$price <- as.numeric(total_g_price$price)
total_g_price$'전용면적(㎡)' <- as.numeric(total_g_price$'전용면적(㎡)')

total_g_price <- total_g_price %>%
  mutate(price_per_size = total_g_price$price / total_g_price$'전용면적(㎡)') %>%
  group_by(시군구명, 동리명) %>% 
  summarise(price_per_size = mean(price_per_size))

merge_price_g <- total_g_price %>% 
  left_join(code, by = c('시군구명','동리명'))

#인천광역시
apart_i <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/아파트(매매)_실거래가_20231215151657.xlsx', skip = 16)
df_yunlib_i <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/연립다세대(매매)_실거래가_20231215151729.xlsx', skip = 16)
df_dandok_i <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/단독다가구(매매)_실거래가_20231215151745.xlsx', skip = 16)
df_offi_i <- read_excel('C:/Users/JunYeong.DESKTOP-IOMT4HU/Downloads/오피스텔(매매)_실거래가_20231215 (1).xlsx', skip = 16)

apart_i <- apart_i[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_yunlib_i <- df_yunlib_i[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]
df_dandok_i <- df_dandok_i[,c('시군구', '거래금액(만원)', '대지면적(㎡)')] %>% 
  rename('전용면적(㎡)' = '대지면적(㎡)')
df_offi_i <- df_offi_i[,c('시군구', '거래금액(만원)', '전용면적(㎡)')]

total_i_price <- bind_rows(apart_i, df_yunlib_i, df_dandok_i, df_offi_i)

#시군구명을 정리하고, 교통권역 지역만 추출
total_i_price <- total_i_price %>% 
  separate('시군구', into = c('시', '구', '동'), sep = ' ') %>%
  separate('거래금액(만원)', into = c('blank', 'price'), sep = '      ') %>%
  select(-price) %>%
  rename(price = blank,
         시군구명 = 구,
         동리명 = 동) %>% 
  filter(시군구명 == '중구' | 시군구명 == '동구' | 시군구명 == '남구' | 시군구명 == '연수구' | 시군구명 == '남동구' | 시군구명 == '부평구' | 시군구명 == '계양구' |
           시군구명 == '서구' | 시군구명 == '강화군')


total_i_price[] <-  lapply(total_i_price, function(x) gsub(",", "", x))

total_i_price$price <- as.numeric(total_i_price$price)
total_i_price$'전용면적(㎡)' <- as.numeric(total_i_price$'전용면적(㎡)')

total_i_price <- total_i_price %>%
  mutate(price_per_size = total_i_price$price / total_i_price$'전용면적(㎡)') %>% 
  group_by(시군구명, 동리명) %>% 
  summarise(price_per_size = mean(price_per_size))

merge_price_i <- total_i_price %>% 
  left_join(code, by = c('시군구명','동리명'))

#서울, 경기도, 인천 합치기
merge_total <- rbind(merge_price_seoul, merge_price_g, merge_price_i)
merge_total$법정동코드 <- as.numeric(merge_total$법정동코드)
merge_total <- merge_total %>% 
  distinct()

#거주지의 평당 주택가격 merge
df_22_merge <- df_22 %>% 
  left_join(merge_total[,c('price_per_size', '법정동코드')], by = c('start_bup_code' = '법정동코드')) %>% 
  rename(price_per_size_start_만원 = price_per_size)

#직장지의 평당 주택가격 merge
df_22_merge <- df_22_merge %>% 
  left_join(merge_total[,c('price_per_size', '법정동코드')], by = c('destin_bup_code' = '법정동코드')) %>% 
  rename(price_per_size_dest_만원 = price_per_size)

print(head(df_22_merge),5)

```

## [분석3.1] 주택가격과 통근시간의 관계가 어떻게 되는가? {.tabset}

### 주택유형 데이터 분석

1: 아파트, 2:연립주택(빌라), 3: 다세대/다가구, 4: 단독주택, 5: 오피스텔(주상복합), 97: 기타

주택유형은 주택가격과도 연관이 있을 수 있다고 판단해, 자 소득계층별로 분리하여 분석을 진행했다.
고소득층은 아파트에 많이 살고, 저소득층은 아파트에 덜 사는 등의 특징이 나타날 수 있어서, 그래프를 그려보았다. 모든 계층에서 아파트를 가장 많이 살았으며, 특히 고소득층에서 아파트 거주 비율이 가장 높은 것을 확인할 수 있고, 그 다음으로는 연립주택이 높았다. 고소득층에서 저소득층으로 갈 수록 아파트 거주 비율이 낮아지는 것을 확인할 수 있었다.

상자그림을 봤을 때 단독주택에 거주하는 경우 통근시간이 가장 높았다.

상관계수는 -0.03으로 매우 약한 음의 상관관계를 가진다.

```{r warning = FALSE, message = FALSE}
df_22$list_home <- as.character(df_22$list_home)
df_22_home <- df_22 %>% 
  group_by(income_status, list_home ) %>% 
  summarise(n = n(), .groups = 'drop')

ds <- data.frame(
  주택유형 = c('아파트', '연립주택', '다세대/다가구', '단독주택', '오피스텔', '기타'),
  저소득층 = c(1113, 648, 649, 140, 280, 29),
  중소득층 = c(2738, 873, 600, 215, 325, 20),
  고소득층 = c(5913, 969, 532, 286, 286, 33)
)

ds <- ds %>%
  mutate(저소득층 = (저소득층 / sum(저소득층)) * 100,
         중소득층 = (중소득층 / sum(중소득층)) * 100,
         고소득층 = (고소득층 / sum(고소득층)) * 100)

ds <- ds %>%
  tidyr::gather(key = 'income', value = '비율', -주택유형)

# 그래프 그리기
ggplot(ds, aes(x = 비율, y = 주택유형, fill = income)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  labs(title = '소득계층별 각 주택유형 거주자 비율',
       x = '거주자 비율(%)', y = NULL) +
  theme(legend.position = 'top', legend.box = 'horizontal', plot.title = element_text(size=25, face='bold',colour='steelblue')) +
  scale_fill_manual(values = c('#003f5c', '#ff6361','#ffa600'),
                    labels = c('고소득층', '중소득층', '저소득층'))

ggplot(df_22, aes(x = list_home, y = commute_time, fill = list_home))+
  geom_boxplot()+
  ggtitle('거주 주택유형과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택유형", y = "통근시간")+
  scale_fill_brewer(palette = "Set3")


df_22$list_home <- as.numeric(df_22$list_home)
print(cor.test(df_22$list_home, df_22$commute_time))
```

<br>
<br>

### 면적당 평균 주택가격의 공간 분포

법정동코드의 문제로 맵핑은 시군구 단위로 진행했다.

분석2.2에서 직장이 많이 존재하는 강남구에서 평당 평균 주택가격이 가장 높은 모습을 볼 수 있다. 그 다음으로 과천시, 서초구가 높은 주택가격을 보였다.
경기도 과천시는 분석2.2에서 전 소득계층 모두 가장 적은 주거를 가졌었다. 이는 높은 주택가격때문으로 해석할 수 있다.

분석결과 보통 직장이 많이 존재하는 서울 3대 업무지역에서 주택가격이 높았다.
반면, 인천광역시 동구, 계양구, 남동구가 가장 낮은 주택가격을 보였다. 전반적으로 인천광역시 지역의 주택가격이 낮았다.

```{r warning = FALSE, message = FALSE}
library(sf)

seoul_map <-  st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/서울/LARD_ADM_SECT_SGG_11.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
gyeongi_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/경기/LARD_ADM_SECT_SGG_41.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)
incheon_map <- st_read("C:/Users/JunYeong.DESKTOP-IOMT4HU/Desktop/개인통행실태조사(2021년기준)/시군구맵/인천/LARD_ADM_SECT_SGG_28.shp", options = 'ENCODING=cp949') %>%
 st_transform(4326)

gyeongi_map <- gyeongi_map %>%
  filter(SGG_NM == '성남시 수정구' | SGG_NM == '성남시 중원구' | SGG_NM == '성남시 분당구' | SGG_NM == '의정부시' | SGG_NM == '안양시만안구' | SGG_NM == '안양시동안구' | SGG_NM == '부천시' |
         SGG_NM == '광명시' | SGG_NM == '고양시덕양구' | SGG_NM == '고양시일산동구' | SGG_NM == '고양시일산서구' | SGG_NM == '과천시' | SGG_NM == '성남시 중원구' | SGG_NM == '구리시' | 
         SGG_NM == '남양주시' | SGG_NM == '하남시' | SGG_NM == '김포시' | SGG_NM == '양주시')

incheon_map <- incheon_map %>%
  filter(SGG_NM == '중구' | SGG_NM == '동구' | SGG_NM == '남구' | SGG_NM == '연수구' | SGG_NM == '남동구' | SGG_NM == '부평구' | SGG_NM == '계양구'|
         SGG_NM == '서구' | SGG_NM == '강화군')

map_list <- list(gyeongi_map, seoul_map, incheon_map)

# 모든 데이터를 하나로 합치기
combined_map <- do.call(rbind, map_list) %>% 
  mutate(SGG_NM = case_when(
    SGG_NM == '고양시일산서구' ~ '고양시 일산서구',
    SGG_NM == '고양시일산동구' ~ '고양시 일산동구',
    SGG_NM == '고양시덕양구' ~ '고양시 덕양구',
    SGG_NM == '안양시동안구' ~ '안양시 동안구',
    SGG_NM == '안양시만안구' ~ '안양시 만안구',
    TRUE ~ SGG_NM
  ))

df_22_merge_startmean <- df_22_merge %>% 
  group_by(출발_시도, 출발_시군구) %>% 
  summarise(mean = mean(price_per_size_start_만원, na.rm = T)) %>% 
  mutate(출발_시군구 = case_when(
    출발_시군구 == '중구' & 출발_시도 == '인천광역시' ~ '인천중구',
    TRUE ~ 출발_시군구
  ))

merged_map <- merge(combined_map, df_22_merge_startmean, by.x = "SGG_NM", by.y = "출발_시군구", all.x = TRUE)
merged_map_sf <- st_as_sf(merged_map)
#상위 3개 레이블링
centroid <- st_centroid(head(merged_map_sf[order(merged_map_sf$mean, decreasing = T),],3))

# natural break 나누기
merged_map_sf$interval <- cut(merged_map_sf$mean, breaks = 5)

# 주거지에 해당하는 면적당 평균 주택가격 분포를 Natural break로 급간을 나눠서 맵핑
ggplot() +
  geom_sf(data = merged_map_sf, aes(fill = interval), lwd = 0.05) +
  scale_fill_brewer(name = "면적당 평균 주택가격", palette = "GnBu", na.value = "grey", guide = "legend") +
  labs(title = "시군구별 면적당 평균 주택가격 분포") +
  theme(plot.title = element_text(size = 25, face = 'bold', colour = 'steelblue')) +
  geom_text(data = data.frame(centroid), aes(label = SGG_NM, x = st_coordinates(centroid)[, "X"], y = st_coordinates(centroid)[, "Y"]),
            size = 2.3, color = "black")

print(head(merged_map_sf[order(merged_map_sf$mean, decreasing = T),],5))
print(head(merged_map_sf[order(merged_map_sf$mean),],5))

```

<br>
<br>

### 주거지의 주택가격과 통근시간 분석

주거지 평당 주택가격의 경우 극단치를 제외하고는 8천만원을 중심으로 정규분포의 형태를 보이고 있다.
주거지 평당 주택가격과 통근시간에 대한 산점도를 분석한 결과, 주거지 면적당 평균 주택가격이 올라갈 수록 통근시간은 감소하는 모습을 보였다.
이는 직장지 주변으로 직주근접형 생활권 형성 및 지속적인 주택공급으로 주택 수요가 충족되었기 때문으로 해석된다.

```{r warning = FALSE, message = FALSE}
#히스토그램
ggplot(df_22_merge, aes(price_per_size_start_만원))+
  geom_histogram(bins = 30, fill = 'hotpink2', alpha = 0.7)+
  ggtitle('거주지 주택가격 분포')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "count")

#산점도
ggplot(df_22_merge, aes(x = price_per_size_start_만원, y = commute_time))+
  geom_point(col = 'cornflowerblue', alpha = 0.7)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('주거지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('상관계수 : ', cor(df_22_merge$price_per_size_start_만원, df_22_merge$commute_time, use = "complete.obs"))


```

<br>
<br>

### 직장지의 주택가격과 통근시간 분석

직장지 주택가격은 1억을 기준으로 정규분포의 형태와 가깝게 형성되어 있는 것을 확인할 수 있다. 
직장지 주택가격과 통근시간에 대해 산점도를 그려본 결과, 직장지의 면적당 평균 주택가격이 높아질 수록 통근시간이 높아지는 경향성을 가졌다. 6억이상의 outlier가 있긴 하지만, 이를 제외하고도 같은 경향성을 가졌다. 직장지의 주택가격이 높으면, 사람들이 직장지 근처에 살기가 어려워지기 때문에 통근시간이 높아지는 것으로 해석된다. 상관계수도 약 0.20으로 개인통행실태조사 내에 존재하는 변수보다 높은 양의 상관관계를 가졌다.

```{r warning = FALSE, message = FALSE}
#히스토그램
ggplot(df_22_merge, aes(price_per_size_dest_만원))+
  geom_histogram(bins = 30, fill = 'hotpink2', alpha = 0.7)+
  ggtitle('직장지 주택가격 분포')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "count")+
  coord_cartesian(xlim = c(0,2500))

#산점도
ggplot(df_22_merge, aes(x = price_per_size_dest_만원, y = commute_time))+
  geom_point(col = 'cornflowerblue', alpha = 0.7)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('직장지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('상관계수 : ', cor(df_22_merge$price_per_size_dest_만원, df_22_merge$commute_time, use = "complete.obs"))

```

<br>

## [분석3.2] 소득계층별 주택가격 영향력 차이 분석 {.tabset}

### 저소득층

저소득층의 경우 거주지 상관계수는 -0.06, 직장지 상관계수 0.20을 가졌다.
저소득층은 주택가격에 가장 민감하게 받을 것으로 예상되었으나, 중소득층보다 직장지의 주택가격에 덜 민감하게 통근시간에 반응했고, 고소득층보다 주거지의 주택가격에 덜 민감하게 반응했다.

```{r warning = FALSE, message = FALSE}
df_22_merge_loww <- df_22_merge %>% 
  filter(income_status == '저소득층')

#산점도 - 주거지
ggplot(df_22_merge_loww, aes(x = price_per_size_start_만원, y = commute_time))+
  geom_point(col = 'darkcyan', pch = 15, alpha = 0.8)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('저소득층 주거지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('거주지 상관계수 : ', cor(df_22_merge_loww$price_per_size_start_만원, df_22_merge_loww$commute_time, use = "complete.obs"), '\n')

#산점도 - 직장지
ggplot(df_22_merge_loww, aes(x = price_per_size_dest_만원, y = commute_time))+
  geom_point(col = 'darkcyan', pch = 15, alpha = 0.8)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('저소득층 직장지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('직장지 상관계수 : ', cor(df_22_merge_loww$price_per_size_dest_만원, df_22_merge_loww$commute_time, use = "complete.obs"))

```

<br>
<br>

### 중소득층

중소득층의 경우 거주지 상관계수가 -0.05로 소득계층 중 가장 낮은 상관계수를 가졌고, 직장지 상관계수는 0.22로 가장 높은 상관계수를 가졌다.
중소득층의 경우 저소득층보다 경제적으로 여유가 있기 때문에 통근시간을 줄이기 위해서 직장이 많이 위치한 지역으로 거주지를 설정할 가능성이 높다. 그러다 보니 직장지의 주택가격이 올라갈 수록 통근시간이 더 민감하게 움직일 수 있다.

```{r warning = FALSE, message = FALSE}
df_22_merge_medii <- df_22_merge %>% 
  filter(income_status == '중소득층')

#산점도 - 거주지
ggplot(df_22_merge_medii, aes(x = price_per_size_start_만원, y = commute_time))+
  geom_point(col = 'darkorange4', pch = 16, alpha = 0.8)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('중소득층 거주지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('거주지 상관계수 : ', cor(df_22_merge_medii$price_per_size_start_만원, df_22_merge_medii$commute_time, use = "complete.obs"), '\n')

#산점도 - 직장지
ggplot(df_22_merge_medii, aes(x = price_per_size_dest_만원, y = commute_time))+
  geom_point(col = 'darkorange4', pch = 16, alpha = 0.8)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('중소득층 직장지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('직장지 상관계수 : ', cor(df_22_merge_medii$price_per_size_dest_만원, df_22_merge_medii$commute_time, use = "complete.obs"))

```

<br>
<br>

### 고소득층

고소득층의 경우 거주지 상관계수가 -0.08로 소득계층 중 가장 높은 상관계수를 가졌고, 직장지 상관계수는 0.17로 가장 낮은 상관계수를 가졌다.
고소득층의 경우 거주지를 선택할 때 주택가격보다도 어메니티 요소를 더 많이 고려할 수 있다. 그렇기 때문에 직장지 보다는 거주지의 주택가격에 더 민감할 수 있다.

```{r warning = FALSE, message = FALSE}
df_22_merge_highh <- df_22_merge %>% 
  filter(income_status == '고소득층')

#산점도 - 거주지
ggplot(df_22_merge_highh, aes(x = price_per_size_start_만원, y = commute_time))+
  geom_point(col = 'cadetblue4', pch = 17, alpha = 0.7)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('고소득층 거주지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue'))+
  labs(x = "주택가격", y = "통근시간")

cat('거주지 상관계수 : ', cor(df_22_merge_highh$price_per_size_start_만원, df_22_merge_highh$commute_time, use = "complete.obs"), '\n')

#산점도 - 직장지
ggplot(df_22_merge_highh, aes(x = price_per_size_dest_만원, y = commute_time)) +
  geom_point(col = 'cadetblue4', pch = 17, alpha = 0.7)+
  stat_smooth(method = 'lm',se = F, color='red')+
  ggtitle('고소득층 직장지 주택가격과 통근시간 비교')+
  theme(plot.title = element_text(size=25, face='bold',colour='steelblue')) +
  labs(x = "주택가격", y = "통근시간")

cat('직장지 상관계수 : ', cor(df_22_merge_highh$price_per_size_dest_만원, df_22_merge_highh$commute_time, use = "complete.obs"))

```

<br>
<br>



# [결론] 결론 및 한계점

<br>

"결론: 2021 개인통행실태조사 데이터를 주로 활용하여 수도권을 중심으로 통근시간 영향요인 분석을 진행했다. 분석질문으로 3가지를 선정했었는데, 각각의 질문에 대한 결론을 내렸다.

1. 통근시간에 영향을 미치는 요인은 무엇이 있고 통근시간과의 관계는 어떻게 되는가?: 

통근시간 영향요인 분석 선행연구에서 주로 사용했던 개인통행실태조사 내에 존재하는 설명변수를 사용한 결과, 여자일 수록 통근시간이 낮아지고, 나이가 많을 수록 통근시간이 낮아지고, 가구원 수가 많을 수록 통근시간이 낮아지고, 운전면허가 없을 수록 통근시간이 높아지고, 서울에 거주할 수록 통근시간이 낮아지는 경향성을 보였다. 개인의 직업이 전문직, 사무직, 관리직일 수록 통근시간이 높았으며, 주요 이용 교통수단이 지하철, 버스와 같은 대중교통일 때 높은 통근시간을 보였다.
상관계수는 성별 변수가 가장 높은 모습을 보였다. 개인의 여러가지 특성 중 성별이 가장 중요한 역할을 했음을 알 수 있다.

2. 소득계층별 평균 통근시간은 어떻게 다르며, 주거지와 직장지의 분포는 어떻게 다른가?:

개인통행실태조사통계적으로 저소득층이 중, 고소득층에 비해 평균 통근시간이 낮은 것으로 나타났다. 모든 소득계층에서 직장은 서울 3대 업무지구에 집중적으로 몰려있었다. 주거지의 경우에는 소득계층별로 상이한 모습을 보였는데, 고소득층으로 갈 수록 직장지가 많이 분포해있는 3대 업무지구쪽으로 옮겨가는 모습을 볼 수 있었다. 그러나 직장지와 주거지의 분포가 가장 다른 저소득층이 고소득층에 비해 평균 통근시간이 낮은 이유는 추가적인 분석이 필요할 거 같다.

3. 주거지와 직장지의 주택가격에 따라 통근시간은 어떻게 달라지며 저소득층일 수록 주택가격이 통근시간에 미치는 영향력이 큰가?:

주택 실거래가 데이터를 개인통행실태조사 데이터와 합쳐 분석을 진행한 결과 직장지가 많이 위치해 있던 서울 3대 업무지구의 주택가격이 주로 높은 모습을 보였다. 주거지의 주택가격이 올라갈 수록 통근시간은 낮아지는 모습을 보였고, 직장지의 주택가격이 높아질 수록 통근시간이 높아지는 경향을 보였다. 직장지의 주택가격이 통근시간에 영향을 미치는 가장 주요한 변수로 볼 수 있었다. 소득계층별로 살펴봤을 때, 고소득층은 통근시간에 있어서 주거지의 주택가격에 가장 민감했고, 중소득층은 직장지의 주택가격에 가장 민감한 경향을 보였다. 고소득층은 어메니티 요소가 주택을 결정하는 데에 있어 중요하고, 중소득층은 통근시간을 줄이기 위해 직장이 많이 위치한 지역에 주거를 하려고 하는 경향이 있다고 해석할 수 있다.

<br>
한계점: 회귀분석을 진행한 것이 아니라, 설명변수 각각을 통근시간과 비교한 것이기 때문에 통근시간에 미치는 영향이 정확하지 않을 수 있다. 개인통행실태조사 내에 존재하는 설명변수들의 경우 대부분 회귀분석 시 통제변수로 이용하는 것들이기 때문에 통근시간과는 직접적인 관련이 없을 수 있다. 통근시간에 영향을 미치는 주요요인으로 주택가격 변수를 만들었으나, 이 외에도 인구밀도, 사업체 수와 같은 데이터도 추가해서 분석을 하고, 주택가격을 결정하는 요인들에 관한 변수도 추가해서 분석하면 더 정확한 결과를 얻을 수 있었을 것이다.
"


<br>
<br>


# [참고문헌] 참고문헌

<br>

- Kim, J. H. "The Impacts of Commuting Time on Work Activities and Health Status." KLIPS Conference. 2016.
- 윤병조, 김연규, and 임한주. "광역권 통근시간 만족도 영향요인 분석에 따른 대중교통 이용 활성화 방안연구." 한국재난정보학회논문집 19.3 (2023): 729-736.
- 하재현, and 이수기. "개인의 생애주기 단계에 따른 통근시간 영향요인 분석--2010 년 수도권 가구통행실태조사자료를 중심으로." 국토계획 52.4 (2017).
- 장재민, 이병호, and 고준호. "근로자의 통근시간 만족도 결정요인 연구: 경기도 거주자를 중심으로." 대한교통학회지 37.4 (2019): 290-301.

<br>
<br>
