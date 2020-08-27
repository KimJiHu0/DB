# MapReduce

> - aggregate framework가 처리하지 못하는 복잡한 집계를 처리할 수 있어.
> - javascript function을 사용해서 작업을 처리해
> - 분산 처리가 가능해
>
> 
>
> 예를 들어서 name은 name끼리, kor은 kor끼리, eng는 eng끼리, math는 math끼리 그리고 sum 합계까지 구하는 문제가 있을 때에는 aggregate로는 처리하지 못해서 MapReduce를 사용해.
>
> 
>
> ### Map
>
> ```sql
> -- Map은 내가 출력할 것들(사용할 것들)을 가지고 오는거야.
> -- screo에서 grouping을 해준다고 보면 돼.
> function myMap(){
> 	-- Collection 이름이 score인 것을 사용할거야.
> 	-- name, kor, eng, test를 사용할거야.
> 	-- this. 을 통해서 가져오는거야
> 	emit(this.score, {name:this.name, kor:this.kor, eng:this.eng, test:this.test})
> }
> ```
>
> 
>
> 
>
> ### Reduce
>
> ```sql
> -- myReduce라는 함수는 쓸건데 key와 values를 파라미터로 받을거야.
> -- myMap에서 찾아온 key와 values라고 볼 수 있어.
> function myReduce(key, values){
> 	-- result라는 변수에 name, kor, eng, total 배열을 선언해줄거야
> 	var result = {name:new Array(), kor:new Array(), eng:new Array(), total:new 		Array()};
> 	-- 파라미터로 받아온 values만큼 for문을 돌릴건데 val을 파라미터로 받을거야.
> 	values.forEach(function(val){
>         -- 만일 test의 val이 final이라면 아래의 구문들을 실행해줄건데
> 		if(val.test=='final'){
>             -- total의 값에 kor과 eng의 val을 더해준 값을 넣어줄거고
> 			result.total += val.kor + val.eng + " ";
>             -- result에 name의 값에 name의 val을 넣어줄거고
> 			result.name += val.name + " ";
>             -- result의 kor에 kor의 val을 넣어줄거고
> 			result.kor += val.kor+ " ";
>             -- result의 eng에 eng의 val을 넣어줄거야
> 			result.eng += val.eng+ " ";
> 		}
> 	})
> 	-- 그리고 result를 return할거야
> 	return result;
> }
> ```
>
> 
>
> 
>
> ### 실행
>
> ```sql
> -- score에 mapReduce를 실행할건데 myMap과 myReduce 함수를 넣어줬어.
> -- 그리고 출력될 out은 myRes라는 새로운 Collection을 만들어서 넣어줄거야.
> db.score.mapReduce(myMap, myReduce, {out:{replace:'myRes'}})
> ```