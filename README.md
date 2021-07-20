# mongoDB_study

본 저장소는 mongoDB에 대한 자료 조사 내용을 가지고 있다. 

# MongoDB 란?
MongoDB는 C++로 작성된 오픈소스 문서지향(Document-Oriented)적 Cross-platform 데이터 베이스이며, 뛰어난 확장성과 성능을 자랑한다. 또한, 2021년 기준 전체 DBMS 중 5위를 차지하고 있고, NoSQL 중에서는 1위를 유지할 정도로 많은 인기를 누리고 있다. 

![rank](./images/DBMS_rank.png)

## DBMS에서 문서지향(Document-Oriented)은 무엇을 의미하는 걸까?

문서 지향적 데이터 베이스는 JSON 유사 형식의 문서로 데이터를 저장 및 쿼리하도록 설계된 비관계형 데이터베이스 유형이다.
```
예시 :
[
    {
        "year" : 2013,
        "title" : "Turn It Down, Or Else!",
        "info" : {
            "directors" : [ "Alice Smith", "Bob Jones"],
            "release_date" : "2013-01-18T00:00:00Z",
            "rating" : 6.2,
            "genres" : ["Comedy", "Drama"],
            "image_url" : "http://ia.media-imdb.com/images/N/O9ERWAU7FS797AJ7LU8HN09AMUP908RLlo5JF90EWR7LJKQ7@@._V1_SX400_.jpg",
            "plot" : "A rock band plays their music at high volumes, annoying the neighbors.",
            "actors" : ["David Matthewman", "Jonathan G. Neff"]
        }
    },
    {
        "year": 2015,
        "title": "The Big New Movie",
        "info": {
            "plot": "Nothing happens at all.",
            "rating": 0
        }
    }
]
```

## MongoDB의 Collection
Collection은 MongoDB Document의 그룹이다. Elasticsearch와 매칭시켜서 보면 index와 동일하다고 보면 된다. 

## 장점
 - Schema-less : schema가 없다는 것은 key-value 모델로도 볼 수 있다는 장점이 있다. 
 - 각 객체 구조가 뚜렷하다.
 - 복잡한 JOIN이 없다. 
 - 문서지향적 Query Language를 사용하여 강력한 성능을 제공한다. 
 - 어플리케이션에서 사용되는 객체를 데이터베이트세 추가할 때, Conversion / Mapping이 불필요하다. 

## 단점 
 - 복잡한 쿼리 사용 불가능
 - 메모리 사용량이 큰 편이다. 
 - 데이터 일관성이 보장되지 않는다. 
# 출처
 - aws 문서 : https://aws.amazon.com/ko/nosql/document/
 - MongoDB 전반적인 내용 : https://velopert.com/436
 - MongoDB 장단점 : https://elky84.github.io/2018/09/26/mongodb/
