# TIL

Today I Learn

**문제**
![Visual Studio Code](/img/x%EB%A7%8C%ED%81%BC.png)

**풀이**

```
class Solution {
    public long[] solution(int x, int n) {
        long[] answer = new long[n];

        //x 만큼 증가하고 n은 갯수이다
        //n을 반복문돌릴때 length로 주면됨
        int sum=x;
        for(int i=0;i<n;i++){
            answer[i]=sum;
            sum+=x;
        }

        return answer;
    }
}
```

\*깨달은점:
long[] answer = {};
이런식으로 작업했었는데 반드시 배열에는long[] answer= new long[n] 이런식으로 배열의 공간을 지정해줘야한다.
