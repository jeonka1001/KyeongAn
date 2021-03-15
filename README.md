# Written By JeonKa
## 주의사항
1. 최대값을 보고 ```long``` 또는 ```int``` 를 결정한다.


## 수학
> 주로 문제의 규칙을 찾은 후 규칙대로 구현하는 문제들  
> 구현에는 크게 어려움이 없으나 규칙을 찾고 설계하는것이 어렵다.
#### 대표유형
책 페이지 https://www.acmicpc.net/problem/1019  
제곱 ㄴㄴ 수 https://www.acmicpc.net/problem/1016  
1. **Check** 용 배열을 선언할 시 ```max``` 와 ```min``` 의 값이 주어진 경우 인덱싱을 하여 배열의 크기를 ```max-min+1```로 선언한다.  
	_이는 배열 사용 시 ```접근 인덱스 - min``` 을 해서 사용한다._


## 트리
1. **트리의 지름** 을 구하는 방법은 **임의의 정점에서 가장 먼 정점** 을 구한 후 **구해진 정점에서 가장 먼 정점** 까지의 **거리** 가 **트리의 지름** 이다.


## 유클리드 호제법 ( GCD ) 
> 두 수의 최대 공약수를 구하는 알고리즘
1. 두 수중 작은 수가 0이 될 때 까지 나머지 연산을 한다.
2. 기본적으로 a > b 를 전제로 하나, b > a 로 입력값이 들어와도 나머지 연산을 통해 수가 뒤집어지므로 크게 상관은 없다.
```
int gcd (int A, int B ){
	int a = A;
	int b = B;
	while(b != a ){
		int r = a % b;
		a = b;
		b = r;
	}
	return a;
}
```
#### 최소 공배수(LCM) 구하기
1. 입력받은 두 수와 최대공약수를 통해 구할 수 있다.
```
int LCM(int A, int B, int gcd){
	return A * B / gcd;
}
```

- UpperBound : 상한의 최저/ LowerBound : 하한의 최고  
	( Tree의 ceilingEntry() 를 통해 얻을 수 있다.)
- 실수의 값은 ```==``` 로 비교할 수 없다. 따라서 **이분탐색** 을 할 경우 ```while(left <= right)``` 와 같은 코드가 아닌 ```for문``` 을 통해 이분탐색을 해야한다. ```for문``` 의 최대 반복 횟수는 **오차 허용 범위 * 0.1 의 역수까지** 이다.  
Ex) 오차범위가 10^-3 일 경우 **10^-3 * 10^-1 = 10^-4 의 역수는 10000 ** 로 한다!!
- 가중치가 있는 단방향 그래프는 편도로 왔다갔다 보다 반대 방향의 그래프를 하나 더 만들어서 그래프를 총 2개를 이용해서  왔다갔다 거리를 측정하는게 훨 속도가 빠르다.  

## union find ( 상호 배타적 집합 )
이 알고리즘은 **원소 a 와 b사이의 관계** 에 대해 알아보고자 할 때 사용한다.  
즉, **a가 있는 집단에 b가 있는지, b가 있는 집단에 a가 있는지** 를 알아보기 위해 사용한다.   
- a 와 b의 관계를 알아보기 위해 사용하는 ```find()``` 함수는 다음과 같다.
```
int find(a){
	if(a == parent[a])
		return a;
	else
		return parent[a] = find(parent[a]); // 경로 압축
}
```
- a와 b의 집합을 합칠 때 사용하는 ```union()``` 다음과 같다. (-> 각 집합의 weight(rank)를 유의하여 합쳐야 한다)
```
void union(int a, int b){
	a = find(a);
	b = find(b);
	if(a == b) // 두 수의 부모가 같다면 
		return ; // 함수 종료
		
	// 두 수의 weight (rank)를 비교한다.
	if(rank[a] > rank[b]){  
		parent[b] = a; 		// rank 가 작은 집합의 부모에 다른 수를 집어넣는다. 
		rank[a] += rank[b];	// 랭크도 같이 ! ( 랭크는 랭크가 큰 집합의 부모에 작은 집합의 부모의 랭크를 더한다)
	}
	else{
		parent[a] = b;
		rank[b] += rank[a];	
	}
}
```
