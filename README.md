<img src="kage@ddsryu/btqS4M9NHVH/6t8KS9RWWViuz7cjSKQos1/img.png" width="40%">

## **B-Tree**

1월 8일 부터 1월 13일까지, B - Tree와 B+ - Tree를 구현하는 프로젝트를 진행하고 있다.

오늘(1월 11일)을 시작으로 한 주 동안 B - Tree의 원리, 삽입 그리고 삭제가 어떻게 이루어지는지에 대해 알아볼 것이다.

B - Tree 관련 포스팅들은 \[**Introductions to Algorithms** (Third Edition), Tomas H. Corman\]의 내용을 토대로 부족한 설명들과 예시를 덧 붙일 예정이다.

[##_Image|kage@mgSY5/btqTcQQ4D0v/K9K1p21plnfEdnRBq6pcHK/img.png|alignCenter|data-origin-width="0" data-origin-height="0" width="301" height="NaN" data-ke-mobilestyle="widthContent"|이해하기 위해 노력한 흔적들||_##]

### **들어가기 전에**

 B - Tree를 공부하며, 인터넷의 여러 강의들 그리고 블로그들을 참고했다. 그런데, B - Tree를 구현하는 방법이 조금씩 달라서 이해하는데 혼란스러웠던 점이 있다. 특히, 대부분의 자료들에서 삽입에 대한 설명보다 삭제에 대한 설명이 부족하다고 느꼈다. 그래서 B - Tree 포스팅에서는 삭제를 조금더 꼼꼼히, 자세하게 다루어 볼 예정이다.

아래는 내가 공부를 하며 책 외에 참고한 자료들이다.

책의 내용이 너무 이해하기 힘들어 다른 인터넷 자료들을 찾아 다녔었다. 하지만, 책 만큼 정확하게 모든 경우들을 다루는 자료는 찾지 못했다. 인터넷의 자료들이 중구남방이라는 느낌을 받았다. 반면, 책은 논리적이고 일관적이라는 것을 깨닫고 책의 로직을 이해하는데 온 힘을 쏟았다.

-   Abdul Bari 교수님의 10.2 B Trees and B+ Trees. How they are useful in Databases [click](https://www.youtube.com/watch?v=aZjYr87r1b8)
    -   책의 방식과 다른 방법을 소개해 준다. 특히, B-Tree를 사용하는 이유, 어떤 장점이 있는지에 대해 깨달을 수 있었다. 삭제에 관한 영상이 없어 굉장히 아쉬웠다. 삭제에 대해서도 설명해 주셨다면, 이 방식으로 구현해 보았을 것 같다.
-   Deletion from a B-tree [click](https://www.programiz.com/dsa/deletion-from-a-b-tree)
    -   책에는 시각적인 자료가 조금 부족한 편인데, B-Tree 삭제 입문을 하는데 큰 도움을 받았다. 하지만, 위의 강의와 마찬가지로 책과 약간 다른 방식을 소개해주며, 이 페이지만 봐서는 코드로 구현하기가 참 힘들다.
-   B-Tree display [click](https://www.cs.usfca.edu/~galles/visualization/BTree.html)
    -   B-Tree를 시각적으로 구성해주는 사이트이다. B-Tree를 공부하는 초기에 내가 공부한 내용이 맞는지 확인하는 용도로 사용했다.

### **B - Tree를 왜 이해해야 할까?**

B-Tree는 디스크나 다른 직접 접근 보조 기억 장치에서 잘 동작하도록 설계된 균형 잡힌 검색 트리이다. Balanced Tree의 종류들 중 하나이다. 이진 트리에서는 노드의 왼쪽과 오른쪽의 균형을 맞추어 주는 매커니즘이 존재하지 않기 때문에, 최악의 경우 모든 노드가 오른쪽으로 쏠리게 된다. 이때 최악의 시간 복잡도는 O(N)이 되게 된다.

[##_Image|kage@lnXrV/btqS90fnDEM/HKEvrKAkz4LBpgYqh1st4k/img.png|alignCenter|data-origin-width="0" data-origin-height="0" width="217" height="NaN" data-ke-mobilestyle="widthContent"|이진트리의 경우 최악의 시간복잡도는 O(N)이다.||_##]

하지만, B-Tree와 같은 Balanced Tree에서는 위의 그림과 같이 노드가 한 쪽으로 쏠리지 않도록, 삽입/삭제시에 재 정렬된다. 이렇게 한쪽 자식에게 값이 몰리지 않도록 재정렬 함으로, 최악의 경우에도 시간 복잡도는 O(log N)이다. 

늘 그렇듯 TradeOff는 존재한다. Balanced Tree에서 삽입/삭제 시 재정렬하는 추가적인 작업을 수행하기 때문에, 삽입/삭제 시에는 이진트리와 같은 일반적인 트리보다 느리다. 삽입/삭제의 성능과 탐색의 성능을 Trade한 것이다.

Balanced Tree의 종류에 B-Tree외에도  Red-Black-Tree가 존재한다. 하지만, B-Tree는 한 개의 노드에 여러개의 key들을 담을 수 있고, Red-Black-Tree에는 한 개의 노드에 한 개의 key만 담을 수 있다. 이러한 이유로 B-Tree의 깊이가 Red-Black\_Tree의 깊이보다 적고, 이는 값을 탐색할 때 이점으로 작용한다.

이러한 이유로 많은 데이터베이스 시스템에서 정보를 저장하는데 B-Tree 혹은 B-Tree 변형을 사용한다.

다음 포스팅에서는 **B-Tree의 규칙**에 대해 알아보자.

틀린 부분이 있을 수 있습니다. 피드백 주시면 고치도록 하겠습니다! 감사합니다.👍
