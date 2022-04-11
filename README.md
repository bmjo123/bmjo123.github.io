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


