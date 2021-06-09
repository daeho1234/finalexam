# 202001618 정보통신공학과 김대호 기말과제

> 모의 담금질을 이용한 parameter estimation
---

### 2차함수 코드구현 테스트

선택함수

![image](https://user-images.githubusercontent.com/80096249/121234855-23d6d780-c8cf-11eb-884c-e655e685a0fc.png)

전역최적해 : -281

![image](https://user-images.githubusercontent.com/80096249/121234712-fdb13780-c8ce-11eb-85e1-18c19e14cdba.png)

#### 최초 제공된 코드의 값

>t:1, a:0.95 niter:100
>lower 0 , upper 31
---

첫번째 시도

![image](https://user-images.githubusercontent.com/80096249/121240969-da3dbb00-c8d5-11eb-927f-52686a165099.png)

결과 : 80.02 (매우큰 오차)

lower upper의 범위 재설정 >> lower : -50, upper : 5

두번째 시도

![image](https://user-images.githubusercontent.com/80096249/121241196-238e0a80-c8d6-11eb-8259-ff8b4e8ab155.png)

결과 : -280.97 (초근사치 검색)

이어진 3회 결과 : -280.96, -280.89, -280.96 (모두 근사치 검색)

조금 더 정확한 코드를 구현하기 위해

a: 0.99 , niter: 1000 으로 조정 


세번째시도

t:1, a:0.99, niter:1000 적용

lower : -50, upper : 5

![image](https://user-images.githubusercontent.com/80096249/121235030-5e407480-c8cf-11eb-9d97-a39db873f366.png)

결과: -280.96

이어진 3회에서 검색된 전역최적해 : -280.99, -280.99, -280.86

조정전보다 평균적으로 더욱 가까운 근사치 검색

![image](https://user-images.githubusercontent.com/80096249/121242344-66041700-c8d7-11eb-8861-6efe91666cee.png)

d와 p0의 값은 변화시키지 않았습니다.

## 4차함수 코드구현 테스트

선택함수

![image](https://user-images.githubusercontent.com/80096249/121238373-00ae2700-c8d3-11eb-9bf4-6f5150889565.png)

![image](https://user-images.githubusercontent.com/80096249/121238536-2a674e00-c8d3-11eb-99cd-265a9d4402db.png)

전역최적해 : -46.412

첫번째 적용 (2차함수에서 적용된 값 그대로 적용)

t:1, a:0.99, niter:1000 적용

lower : -50, upper : 5

![image](https://user-images.githubusercontent.com/80096249/121239409-2b4caf80-c8d4-11eb-993b-09ebf85bbef4.png)

결과 : -46.27
(첫값이 너무 크게 잡혀 lower -10, upper 2로 조정)

조정 후 결과값

![image](https://user-images.githubusercontent.com/80096249/121239704-89799280-c8d4-11eb-97bb-e0ec8ec6f38e.png)

![image](https://user-images.githubusercontent.com/80096249/121240208-0ad12500-c8d5-11eb-95a3-8c7b1f1222d2.png)

결과 : -46.38 (후반부의 등락 변화를 보기 위해 후반부 커브를 확대했습니다.)

(소숫점 자리수와 많은 값들로 커브를 알아볼 수 없어 데이터레이블 제외)

기존 코드가 4차함수에서도 거의 정확한 값을 추정합니다.

### parameter 검증과정

선택함수

![image](https://user-images.githubusercontent.com/80096249/121244779-428e9b80-c8da-11eb-925d-2f6a6b64d715.png)

![image](https://user-images.githubusercontent.com/80096249/121244825-4e7a5d80-c8da-11eb-8947-934163911957.png)

>선택한 함수의 정규분포 형태의 그래프 제작
---
```
import java.util.Random;

public class distribution {

    public static void main(String[] args) {

        int x=0,y;
        Random r = new Random();

        for(int i=-8;i<8;i++) {
            y = 2 * i + 1;
            System.out.print(y + r.nextGaussian() + ", ");
        }
    }
}
```

![image](https://user-images.githubusercontent.com/80096249/121247130-d95c5780-c8dc-11eb-9d22-62a7a6f0681c.png)

위의 코드를 이용하여 정규분포로 분산시킨 그래프

![image](https://user-images.githubusercontent.com/80096249/121294953-3120b000-c929-11eb-81e2-c1f3c05732bc.png)

  | x | 0   | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | y | 0.89    | 3.36   | 6.11   | 6.9  | 8.68  | 12.69  | 14.39   | 15.00  | 17.38  | 18.99  |
  
 다음값들이 주는 기울기 평균 2.01
 
 y = 2.01x + b 값을 기준으로 simulated annealing의 결괏값
 
  | x | 0   | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | y | 0.04    | 2.02   | 4.03   | 6.15  | 8.04  | 10.15  | 12.08   | 14.07  | 16.08  | 18.08  |
  
  (lower를 0부터 1씩올려가며 전역최적해를 구함)
 원래 결과와의 오차를 계산하여 평균을 냄.
 
![image](https://user-images.githubusercontent.com/80096249/121296763-2287c800-c92c-11eb-83fb-c46ff2401bd8.png)

오차평균 : 1.34
b를 1.34로 추정가능

설정한 
추정된 결과 : y = 2.01x + 1.34
원래 결과 : y = 2x + 1

![image](https://user-images.githubusercontent.com/80096249/121297047-ae99ef80-c92c-11eb-83ec-8b47207c00cb.png)

구현된 simulated annealing이 구한 식과 원래 결과의 식이 흡사.

### 데이터를 이용한 curve fitting

다음은 부동산통계처주택 통계부에서 제공된

20년 6월 이후 매달 서울시 일부 아파트 가격 분포의 증가의 평균을 가격지수로 나타낸 표이다.

![image](https://user-images.githubusercontent.com/80096249/121298133-506e0c00-c92e-11eb-95d5-19cc8bd78cc9.png)

  | 연월 | 20/6(+1)   | 20/7(+2)  | 20/8(+3)  | 20/9(+4)  | 20/10(+5)  | 20/11(+6)  | 20/12(+7)  | 21/1(+8)  | 21/2(+9)  | 21/3(+10)  | 21/4(+11)  | 21/5(+12)  |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 가격지수 | 108.9    | 109.7   | 110.1   | 110.4  | 110.6  | 110.8  | 111.1   | 111.5  | 112.1  | 112.5  | 112.9  | 113.3  |
  
  y = ax + b값을 추정하기 위해 기울기의 평균값 계산
  
  0.8, 0.4, 0.3, 0.2, 0.2, 0.3, 0.4, 0.6, 0.4, 0.4, 0.4
  
  평균값 : 0.4
  a를 0.4로 estimation.
  
  y = 0.4x로 simulated annealing에 대입
  
  | 연월 | 20/6(+1)   | 20/7(+2)  | 20/8(+3)  | 20/9(+4)  | 20/10(+5)  | 20/11(+6)  | 20/12(+7)  | 21/1(+8)  | 21/2(+9)  | 21/3(+10)  | 21/4(+11)  | 21/5(+12)  |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 가격지수 | 0.43    | 0.87   | 1.22   | 1.62  | 2.02  | 2.48  | 2.82   | 3.22  | 3.65  | 4.01  | 4.40  | 4.80  |
  
![image](https://user-images.githubusercontent.com/80096249/121301675-8235a180-c933-11eb-8c9b-f3757d0aaabf.png)

위의 값과 차이를 이용한 평균은 109.33 = b로 estimation합니다.

즉 추정된 결과는 y = 0.4x + 109.33

![image](https://user-images.githubusercontent.com/80096249/121302192-48b16600-c934-11eb-91af-dde345202f41.png)

추정된 그래프와 정규분포의 평균 비교


#### 성능분석 및 결과

![image](https://user-images.githubusercontent.com/80096249/121302192-48b16600-c934-11eb-91af-dde345202f41.png)

결과:

잘 설정된 simulated annealing은 검증과정과 실제 그래프를 참조할 때 유사한 curvefitting과 유사한 a,b를 추정할 수 있다고 볼 수있다.

성능분석:

simulated annealing 보다 정규분포표로 나타내진 그래프를 추정할 수 있지만,
likelihood등의 보다 유리한 성능의 코드가 존재한다.
