## [MySQL] MAX 함수는 null은 최대 최소 계산시 무시한다 


오늘 삽질했던 케이스를 공유해본다. 

아래와 같이 company_photo 라는 테이블이 있다 치자.




	company_photo
	id | is_represent |company_name|  register_date|
	----------------------------------------------------
	 1 | Y            |Vista Corp|  2015-10-23   |
	 2 | N            |Vista Corp|  2015-10-22   |
	 3 | N            |Data Corp |  2015-10-11   |
	 4 | N            |Data Corp |  2015-10-12   |
	 5 | Y            |Test Corp |  2015-10-23   |


자아 여기서 회사의 대표 이미지(is_represent 가 'Y'인 값)가 아니면 최신 등록한 최신 company_photo id를 구해야 한다고 생각하자.

(단 id는 auto_increment 로 설정되어 있다.)

아래 처럼 나와야 한다


	company_name| id | 
	------------------
	 Vista Corp| 1 |
	 Data Corp | 4 |
	 Test Corp | 5 |


이걸 굳이 한방의 쿼리로 만들려면 어떨까?

그러면 MySQL은 MAX 함수는 null은 최대 최소값에 포함시키지 않는다는 특징을 이용해서 아래와 같이 쓸수 있다. 


	SELECT 
	 company_name,
	  IF(
		  	MAX(IF(is_represent = 'Y', id , null)) IS NOT NULL,  
		  	MAX(IF(is_represent = 'Y', id , null)),  
		  	MAX(IF(is_represent = 'Y', null , id))
		) AS represent_id
	FROM company_photo
	GROUP BY company_name


1. 제일 앞의 IF는 represent = Y 값중 제일 큰 값이 NULL이 아닐때를 구분해주기 위해서 쓴다.
2. MAX는 최신 ID를 구하기 위해서 사용한다.
3. MAX 안쪽의 IF는 is_represent 값에 따라 MAX 값을 계산할지 말지 결정한다.

SQL이라 가독성이 안좋은데 굳이 이런짓해서 더 안좋게 만들지 말자. 
메모리가 넉넉하다면 그냥 INNER VIEW 사용해서 조회하는 쪽이 더 낫다.

(어차피 전체를 다 뒤지기 때문에 이 쿼리는 인덱스가 있어봐야 크게 소용없다. )

배치에서 코드를 좀 더 좋게하려고 이런 이상한 쿼리를 짜긴 했는데, 쓰지 않을듯 싶다.
