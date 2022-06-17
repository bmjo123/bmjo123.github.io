---
layout: single
title: "유전 알고리즘을 이용한 회귀분석"   
---

# 회귀분석     

회귀분석이란 독립변수가 종속변수에 영향을 미치는 지 알아보고자 할 때 사용하는 분석 방법이다.       
독립변수는 값에 따라 영향을 받지 않는 변수이고, 종속변수는 독립변수에 영향을 받아 값이 바뀌는 변수이다.        


# 유전 알고리즘     

유전 알고리즘은 생성, 교차, 변이 등의 과정을 반복하면서 자식 세대를 형성하고, 이 과정을 반복하며 적절한 해를 찾는다.      
먼저 초기 염색체를 생성해야 한다. 처음에는 값이 없기 때문에 랜덤하게 값을 나타내기도 한다.     
생성된 값으로 원래 식에 얼마나 적절한지 적합도를 계산하고, 적합도를 기반으로 정해놓은 컷에 따라 다음에 살아남을 유전자를 선정한다.     
여기서 룰렛휠이라는 개념이 나오는데, 이 개념의 의미는 적합도를 기반으로 확률적으로 다음 세대에 살아남을 유전자를 정하는 것이다.    
따라서 확률이 높을 수록 다음까지 살아있을 확률이 높다. 그 다음 선택된 유전자들끼리 crossover 즉 교차를 시킨다.    
교차는 부모 해를 바탕으로 자식 해를 만드는 연산을 한다.    
여기서 돌연변이가 나타날 수 있는데 돌연변이는 만들어진 자식 해를 일부 수정하는 것이다.    
이렇게 하는 이유는 두 부모와 완전히 일치하지 않는 해를 만듦으로 세대를 거듭할 수록 특정해에 수렴하는 것을 방지하기 위해서이다.    

# 유전 알고리즘 예제     

먼저 초기해를 생성하는 함수를 만들었다.      
def generate_initial_population(n);     
initial_population = np.random.randint(0, 3,(n, 2, 5))       
return initial_population      
다음으로 적합도 함수를 만들면      
def evalutate_fitness(s);      
x1 = sum(2**(4-i) * s[0][1] for i in range(5))     
x2 = sum(2**(4-i) * s[1][i] for i in range(5))      
constraint_1 = (100*x1 + 50 * x2) <= 2000    
constraint_2 = (10*x1) <= 100    
if constraint_1 and constraint_2;    
  fitness_value = 100 * x1 + 40*x2       
else;       
  fitness_value = 0;     

여기서 두 해를 선택해 교차연산을 수행하는 함수를 정의하면       
def crossover(s1, s2);      
  crosover_point1 = np.random.randint(1, 4)        
  crosover_point2 = np.random.randint(1, 4)        
  child - np.empty((2, 5))       
  child[0][:crossover_point_1] = s1[0][:crossover_point1]      
  child[0][crossover_point_1] = s1[0][crossover_point1]       
  child[1][:crossover_point_2] = s1[1][:crossover_point1]          
  child[1][crossover_point_2] = s1[1][crossover_point1]           
  
돌연변이 연산은 a를 일어날 확률이라고 할 때, x가 1이면 0, 0이면 1로 바꾼다. if는 a확률로 선택하는데 사용된다.       

def mutation(child, a);               
  for row in range(2);                   
    for col in range(5);          
    if np.random.random() < a;        
    child[row, col] = 1 - child [row,col]       
    return child;          
    
함수를 정의했으므로 메인 코드를 정의하면         

current_population = generate_initial_population(n)            
best_score = -1           
for_in range(num_iter - 1);      최대 적합도를 비교해 지금값이 크다면 바꾼다.        
  fitness_value_list = np.array([evaluate_fitness(s) for s in current_population])            
  if fitness_value_list.max() > best_score;         
  best_score = fitness_value_list.max()        
  best_s = current_population[fitness_value_list.argmax()]       
  parents = current_population[np.argsort(-fitness_value_list)]       적합도에 따라 해 선정           
  new_population = parents   새로운 해 정의           
  for_in range(n - n_p):          
    parent_1_idx, parent_2_idx = np.random.choice(n_p, 2, replace = False)            
    parent_1 = parents[parent_1_idx]          
    parent_2 = parents[parent_2_idx]      
    child = crossover(parent_1, parent_2)     자식 생성하기                  
    if np.random.random()<mutation_s_prob;       
      child = mutation(child, mutation_gene_prob)         돌연변이 연산 실행              
      new population = np.vstack([new_population, child.reshape(1, 2, 5])
    
    
    
---
layout: single
title: "포드 풀커슨 알고리즘"   
---

# 포드 풀커슨 알고리즘      

포드 풀커슨 알고리즘은 네트워크 유량 문제 중 하나이다.             
네트워크 유량은 이 유량 네트워크에서 최대의 유량을 구하는 문제이다.          
이때의 유량을 최대 유량이라고 한다. 포드 풀커슨 알고리즘은 이 최대의 유량을 구하는 알고리즘이다.            
네트워크 유량에는 3가지의 특징이 있다.      
첫 번째로는 용량의 제한으로 유량은 간선의 용량을 넘을 수 없다는 것이다.      
두 번째로는 유량의 보존으로 어떤 정점을 기준으로, 정점에서 들어오는 유량과 나가는 유량의 크기가 같아야 한다.           
세 번째로는 유량의 대칭으로 만약 a에서 b로가는 간선에 흐르는 유량은 그 크기의 음수만큼 b에서 a로 흐를 수 있다는 것이다.          
이 알고리즘에서는 source를 시작점, sink를 도착점으로 정한다.           

# 포드 풀커슨 알고리즘 작동방식     

1. 먼저 source에서 sink로 가는 경로를 찾는다. 이 때 경로에 여유 용량이 남아 있어야 한다.       
2. 찾은 경로를 이루는 간선 중에서 용량이 가장 작은 간선을 찾는다.     
3. 찾아낸 경로에 유량을 흘려서 그 간선을 사용한다.       
4. 위와 같은 방법을 더 이상 할 수 없을 때까지 반복한다.     

![제목 없음](https://user-images.githubusercontent.com/101388345/165822983-ab27cd47-7024-45ba-b185-b9047c768cd1.png)

숫자는 용량이다.         
만약 위와 같은 경로가 있다면, 제일 먼저 s->a->b->k라는 경로를 생각할 수 있다. 여기서 s->a가 유량 1이므로 지나간 경로에 다 1을 저장하게 된다.          
다음으로는 s->b->k경로를 생각할 수 있다.           
다음으로 쓰일 경로는 이제 a로는 가지 못하니 b로 갔는데 위의 세 번째 특징에서 말했듯이 방향을 반대로 해서 유량이 흐를 수 있다는 것을 사용해 b->a를 이용할 수 있다.       
다음으로 쓰일 경로가 이제 없기 때문에 알고리즘이 종료된다.          


# 포드 풀커슨 코드          

포드 풀커슨 코드를 구현     

if(!visited.getOrDefault(j, false) && dif.get(i).get(j) > 0) {           
        prev.put(j, i);                
				visited.put(j, true);                  
				q.add(j);               
유량이 간선을 통과하지 않았거나 용량이 0이 아닐 때만 지나갈 수 있다.           

while(bfs(src, dst)) {             
			int pathFlow = INF;             
s에서 k까지 경로가 존재하지 않을 때까지 실행한다.     

for(int j = dst; j != src; j = prev.get(j)) {                 
				int i = prev.get(j);                
				pathFlow = Math.min(pathFlow, dif.get(i).get(j));          
s에서 k까지 경로를 이루는 간선 중 용량이 가장 작은 것을 찾는다.       

			for(int j = dst; j != src; j = prev.get(j)) {               
				int i = prev.get(j);         
위에서 찾은 경로에 유량을 흐르게 한다.           



# 포드 풀커슨 성능분석           

포드 폴커슨 알고리즘은 s에서 k까지 증가경로가 없을 때까지 반복해서 실행되는 알고리즘이다.         
따라서 포드 풀커슨 알고리즘은 네트워크 용량이 정수일 때, 최소 증가경로로 흐를 수 있는 유량이 1이므로 모든 경로를 돌면 실행이 끝마쳐지게 된다.      
또한 포드 풀커슨 알고리즘의 시간복잡도에 대해 알아보면, 포드 풀커슨 알고리즘은 증가 경로를 하나씩 찾을 때마다 전체 유량이 최소 1씩은 늘어나는 것을 사용한다.        
따라서 최대 유량 f에 대해 증가 경로 또한 f번이 최대이고, 여기에 그래프 탐색에 사용한 O(|V| + |E|)를 사용하면 O(|E|f)의 시간이 걸리는 것을 알 수 있다.       
하지만 위와 같이 시간복잡도를 구할 경우 만약 최대 유량이 큰 경우 엄청나게 많은 횟수를 반복해야 할 수 있다.        
그래서 증가경로를 찾을 때 BFS를 사용하면 O(V + E) = O(E)이고, 증가경로는 최대 O(VE)번 찾으므로, 각 간선은 최대 V만큼 포화될 수 있으므로,  O(VE^{2})이 나온다.             
이것을 위에서 나온  O(|E|f)와 비교해 작은 것을 쓰면 그것이 시간복잡도가 된다.       



---
layout: single
title: "허프만 코드 트리와 성능분석"   
---

# 허프만 코드 이진트리    

허프만 코드 이진트리는 텍스트 압축을 위해 사용하는 방법으로 사용 빈도가 높으면 작은 비트로 낮으면 큰 비트로 변환해서 나타낸 트리다.    
출현 빈도를 구했으면 허프만 코드에 입력시키고, 입력시킨 텍스트들이 변환값을 가진다.    

            Node leftChild = mini.MinNode();
            Node rightChild = mini.MinNode();

            plusparent = new Node(leftChild.txt+rightChild.txt,'.');
            plusparent.leftNode=leftChild;
            plusparent.rightNode=rightChild;

            if(mini.isEmpty()) return;

            mini.insert(plusparent);
        }     
위의 코드로 트리의 왼쪽은 0, 오른쪽은 1을 할당한 이진코드의 값은 아래의 사진과 같다.     
![162628647-edce3538-ec7d-47ac-941f-d78a86902451](https://user-images.githubusercontent.com/101388345/162677755-3d278a8b-f775-4e1c-a9d1-65384fd4e61c.png)

따라서 이 이진코드의 허프만 트리를 그려보았다.     
![허프만 트리 구현](https://user-images.githubusercontent.com/101388345/162677853-f5705be7-b398-4bba-bac5-f9348ffa5fe0.png)
위 사진과 같이 사용 빈도가 높은 문자가 트리의 앞부분에 있고 낮은 문자가 트리의 제일 밑부분에 있는 것을 알 수 있다.     



# 허프만 코드 성능분석    

허프만 코드에서 성능분석이란 기존의 텍스트 파일에서 압축하고 난 후의 텍스트 파일이 얼마만큼 줄었는가를 나타내는 것이다.   
기존의 alice 텍스트 파일에서의 문자열의 길이가 107748이었고, ASCII 코드로 나타낼 시 8bit를 곱한 861984bit이다.    
허프만 코드에서는 변형된 값이 가변 길이 코드이다. 이진코드의 값에 각각의 빈도수를 곱해서 더했을때 값이 451520이 나왔다.    
451520 / 861984 *100 = 54 정도 이므로 대략 46% 정도 압축되었다. 

앞에서 언급했듯이 허프만 알고리즘은 압축률이 일정한 것이 아니라 값이 변동이 된다.    
또한 위의 트리를 구현했을 때 이진코드가 들어갈 노드들이 앞에 많이 있음에도 불구하고 코드가 길어진 것 처럼, 알고리즘을 짜는 방식에 따라 이진코드의 값을 줄여서 압축률을 높일 수 있을 것이다.


