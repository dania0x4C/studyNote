[[정렬]]

```
import sys

n = int(sys.stdin.readline())

arr =list(map(int, sys.stdin.readline().split()))

sortArr = list(set(arr))

sortArr.sort()

for i in range(n):

    print(sortArr.index(arr[i]), end="")
```
위의 문제는 해당 값과 비교해서 몇 개의 작은 값이 존재하는지 확인 하는 문제이다
이중 for문을 사용하면 가능하지만 시간 복잡도를 줄이기 위해서 index값을 활용하기로 했다.

but,
시간 초과 문제점 발생-> index()라는 함수는 시간 복잡도가 O(n)이다
그래서 emulate을 사용하여 해결 하려고 했으나 떠오르지 않음

마지막으로 dic을 이용해서 문제 해결

```
import sys

n = int(sys.stdin.readline())

arr =list(map(int, sys.stdin.readline().split()))

sortArr = sorted(list(set(arr)))# list의 중복 값을 없애고 정렬

dic = {sortArr[i] : i for i in range(len(sortArr))} # dic{값 : index}로 값에 따른 index값 설정

for i in arr:

    print(dic[i], end="")# dic[value]는 해당하는 key 값을 반환한다
```
다음과 같이 dic을 활용하여 index값에 접근 할 수 있었다.

