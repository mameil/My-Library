---
description: 'Dynamic Programming : DP'
---

# 동적계획법

특정 범위까지의 값을 구하기 위해서 그것과 다른 범위까지의 값을 이용하여 효율적으로 값을 구하는 알고리즘 설계 기법이다

쉽게 생각하면 답을 재활용하는 것이다

어떤 문제를 풀기 위해 그 문제를 더 작은 문제의 연장선으로 생각하고, 과거에 구한 해를 활용하는 방식의 알고리즘임

동적계획법은 다른 범위까지의 값을 이용하여 효율적으로 값을 구하는 알고리즘이라고 작성해 두었는데, 그 의미는 결국 사용했던 것을 사용하라 라는 의미를 갖는다.

때문에 메모제이션을 사용한다

메모제이션이란? 미리 계산해뒀던 값들을 배열 따위에 저장해두어서 이전의 값들을 계산을 통해서 가져오는 개념이 아니라 계산된 값들을 배열에 넣어두었다가 값들을 가져오는 방식을 의미한다.

만약에 이전의 값들을 계산을 통해서 가져오게 된다면 시간 복잡도가 2^n이 되겠지만

배열에 넣어두었다가 값들을 가져오는 방식은 n이다.

\
\
\
\
\
\
\
\
\
\
