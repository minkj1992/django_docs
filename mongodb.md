t kit오후 2:53
두가지 질문이 있습니다. 1) in 의 경우 여러번 호출하면 똑같지 않나요? 샤드 클러스터 환경에서 장점 일까요?
2) hashed_account_id 는 account_id 와 카디날리티가 동일하지 않나요?
kael msh오후 2:54
ㅔ
man ant오후 2:54
위에 말했는데 놓치신거같아서요~ 복합 인덱스 컬럼이 3개면, 선행 필드 2개 필드가 꼭 있어야 인덱스 사용 되나요? (혹은 선행 필드 1개만 있어도 되나요)
kael msh오후 2:55
임베디드 다큐먼트 입니다
t kit오후 2:56
첫번째 질문을 보강하면 in 안에 여러개의 인덱스 키를 넣으면 내부적으로 여러번 호출하고 각 호출은 인덱스 스캔으로 동작하는것으로 이해 했습니다 이를 쿼리 분리하면 클라이언트에서 in 에 들어가는 배열의 갯수만큼 호출하는것으로 이해되는데 그럼 동일하지 않나 하는 질문입니다.
me poet오후 2:56
질문)사내에도 다양한 nosql db가 사용되는걸로 알고 있는데요. 일반적으로 hbase, redis와 비교해 몽고디비를 썼을 때 장점은 어떤 것이 있을까요? 
andy hj오후 2:57
@man ant
안녕하세요~
꼭 2개 필드가 있어야 할 필요는 없습니다.
예를 들어, "a", "b", "c", 로 구성된 복합인덱스의 경우

"a" 필드를 사용,
"a","b" 필드를 사용,
"a","b","c" 필드를 사용할 경우 모두 사용가능합니다.
metiz jang오후 2:57
질문) 인덱스는 내림차순인데 오름차순으로 정렬하게되면 메모리에서 다시 정렬하게 되나요?
raon paz오후 2:58
질문) 몽고디비도 커버링 인덱스 기법 사용이 효율적일까요?
seo dj오후 2:58
@kael.msh
Q. Document 의 하위 depth node 에 인덱스를 걸면 성능상 좋지 않은걸까요?
A. 인덱스 구조상 일반 document 의 필드에 인덱스를 생성하거나 sub document 의 필드에 인덱스를 생성하는 것은 차이가 없습니다.
혹시 sub document 를 통째로 인덱싱 하는 것을 문의하시는 것이라면 이는 인덱스 leaf node 가 커지는 이슈와 sub document 순서에 영향을 받으므로 주의하셔야 합니다. 이 때문에 sub document 를 통째로 인덱싱 하는 것은 그리 추천드리지 않고 있습니다.
만약 array sub document 에 인덱싱을 하신다면 인덱스의 leaf node 가 상당히 많아질 수 있으니 이 또한 주의해야 합니다.
tory han오후 2:59
질문) ESR rule이 효율적이지 않은 경우가 언제인가요?
t kit오후 3:00
호출당 비용이 낮아지는 메모리 효율관점이군요, 넵 잘 이해했습니다. 😀
man ant오후 3:00
@ andy hj 답변 감사합니다. (3개 필드에 대한 예제를 보니 알겠네요 ㅎ)
t kit오후 3:01
아하 해싱할 때 다른 필드를 포함했군요
seo dj오후 3:01
@me.poet
질문)사내에도 다양한 nosql db가 사용되는걸로 알고 있는데요. 일반적으로 hbase, redis와 비교해 몽고디비를 썼을 때 장점은 어떤 것이 있을까요? 
A. 각각의 솔루션들이 제각각의 장단점을 가지고 있습니다. 문의주신 hbase, redis 와 비교했을때의 mongodb 장점을 말씀드리면,
vs hbase : mongodb 는 세컨더리 인덱스를 지원하기 때문에 필요한 인덱스를 여러개 생성해서 조회하실 수 있습니다.
vs redis : redis 는 메모리 기반이므로 데이터가 휘발성인데 mongodb 는 데이터의 저장을 보장합니다. 또한 redis 에는 세컨더리 인덱스가 없으므로 이 부분또한 장점이 되겠습니다.
t kit오후 3:01
이부분도 이해했습니다. 고맙습니다 🙇
seo dj오후 3:02
@metiz jang
질문) 인덱스는 내림차순인데 오름차순으로 정렬하게되면 메모리에서 다시 정렬하게 되나요?
A. 인덱스의 leaf node 는 양방향 linked list 이므로 내림차순 인덱스로도 오름차순 정렬이 가능합니다.
metiz jang오후 3:02
감사합니다
me poet오후 3:02

@tory han 
Q: ESR rule이 효율적이지 않은 경우가 언제인가요?
A: 첫번째 필드인 이퀄 조건이 카디널리티가 좋지 않을때는 인덱스 효율이 없습니다. 
예를 들어 상태 값이 4개라면 카티널리가 25%이기 때문에 효율이 좋지 않습니다. 따라서, 이퀄 조건은 최대한 카티널티가 좋아야 효율적입니다. 

max im오후 3:07
lookup, project를 쓰는 쿼리에서 ''BufBuilder attempted to grow() to 67108916 bytes, past the 64MB limit.' 라는 오류를 본 적이 있는데요, 
실제 project 되는 필드는 소수였습니다. 이 경우 인덱스에 해당 필드를 추가했다면 문제가 해소 되었을까요?

raymon p오후 3:08
혹시 카디널리티나 선택도를 쉽계 구할 수 있는 방법이 있을까요 ^^?

> 카디널리티는 정량적인 개념이라, 그냥 갯수 크면 된다. 선택도는 

max im오후 3:11
혹시 힌트는 비권장인가요?