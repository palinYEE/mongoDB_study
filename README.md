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

# Docker MongoDB 설치

1. 이미지 다운로드 : `docker pull mongo`

2. 구동 : `docker run --name mongodb_server -v /Users/yoon-yeoungjin/Desktop/git_repository/mongoDB_study/db:/data/db -d -p 27017:27017 mongo`
    - `--name`      : 컨데이너 이름 설정
    - `-v`          : 볼륨을 외부와 연결 --> 위 명령어에서는 db볼륨을 외부로 연결
    - `-d`          : 데몬으로 실행
    - `-p`          : 외부 접속을 위해 포트 연결

3. Bash 접근 및 MongoDB 접속
    - `docker exec -it mongodb_server bash`
    - `mongo`

## 간단 사용 방법

- 데이터베이스 생성
    ```
    > use mongodb_tutorial
    switched to db mongodb_tutorial
    ```
- 현재 사용중인 데이터베이스 확인
    ```
    > db
    mongodb_tutorial
    ```
- 내가 만든 데이터베이스 리스트 및 용량 확인
    ```
    > show dbs
    admin   0.000GB
    config  0.000GB
    local   0.000GB
    ```
    위 결과를 보면 우리는 `mongodb_tutorial` 데이터베이스를 만들었는데 보여지지 않는다. 이유는 데이터베이스를 보려면 최소 한개의 도큐먼트를 추가해야되기 때문이다. 따라서 다음과 같이 입력하면 볼 수 있다. 
    ```
    > db.book.insert({"name" : "MongoDB Tutorial", "author" : "velopert"});
    WriteResult({ "nInserted" : 1 })
    > show dbs
    admin             0.000GB
    config            0.000GB
    local             0.000GB
    mongodb_tutorial  0.000GB
    ```
- 데이터베이스 제거
    ```
    > db.dropDatabase();
    { "ok" : 1 }
    > show dbs
    admin   0.000GB
    config  0.000GB
    local   0.000GB
    ```
- Collection 생성

    | Field     | Type    | 설명                                                                                                                                                                                                                                        |
    |-----------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | capped    | Boolean | 이 값을 true 로 설정하면 capped collection 을 활성화 시킵니다. Capped collection 이란, 고정된 크기(fixed size) 를 가진 컬렉션으로서, size 가 초과되면 가장 오래된 데이터를 덮어씁니다. 이 값을 true로 설정하면 size 값을 꼭 설정해야합니다. |
    | size      | number  | Capped collection 을 위해 해당 컬렉션의 최대 사이즈(maximum size)를 ~ bytes로 지정합니다. |
    | max       | number  | 해당 컬렉션에 추가 할 수 있는 최대 갯수를 설정합니다.|
    - ex_1 : test 데이터베이스에 books 컬렉션을 옵션 없이 생성
        ```
        > db.createCollection("books")
        { "ok" : 1 }
        ```
    - ex_2 : test 데이터베이스에 articles 컬렉션을 옵션과 함께 생성
        ```
        > db.createCollection("articles", { capped: true, size: 6142800, max: 10000 })
        { "ok" : 1 }
        ```
    - ex_3 : 따로 `createCollection()` 메소드를 사용하지 않아도 도큐먼트를 추가하면 자동으로 컬렉션이 생성된다.
        ```
        > db.people.insert({"name" : "yyj"})
        WriteResult({ "nInserted" : 1 })
        ```
    - collection 리스트 확인
        ```
        > show collections
        articles
        books
        people
        ```
- Collection 제거
    ```
    > show collections
    articles
    books
    people
    > db.people.drop()
    true
    > show collections
    articles
    books
    ```
- 도큐먼트 추가 : `db.COLLECTION_NAME.insert(document)`
  - ex_1 : 한개의 도큐먼트를 books 컬렉션에 추가.
    ```
    > db.books.insert({"name" : "mongodb gide", "author" : "yyj"})
    WriteResult({ "nInserted" : 1 })
    ```
  - ex_2 : 두개의 도큐먼트를 books 컬렉션에 추가. 
    ```
    > db.books.insert([
    ... {"name": "book1", "author" : "yyj"},
    ... {"name": "book2", "author" : "yyj"}
    ... ]);
    BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
    })
    ```
  - ex_3 : 컬렉션의 도큐먼트 리스트 확인.
    ```
    > db.books.find()
    { "_id" : ObjectId("60f636174ac0ab80c78b671b"), "name" : "mongodb gide", "author" : "yyj" }
    { "_id" : ObjectId("60f636974ac0ab80c78b671c"), "name" : "book1", "author" : "yyj" }
    { "_id" : ObjectId("60f636974ac0ab80c78b671d"), "name" : "book2", "author" : "yyj" }
    ```

- 도큐먼트 제거 : `db.COLLECTION_NAME.remove(criteria, justOne)`

    | parameter | type     | 설명|
    |-----------|----------|------------------------------|
    | *criteria | document | 삭제 할 데이터의 기준 값 (criteria) 입니다. 이 값이 { } 이면 컬렉션의 모든 데이터를 제거합니다. |
    | justOne   | boolean  | 선택적(Optional) 매개변수이며 이 값이 true 면 1개 의 다큐먼트만 제거합니다. 이 매개변수가 생략되면 기본값은 false 로 서, criteria에 해당되는 모든 다큐먼트를 제거합니다. |

    - ex. books 컬렉션에서 "name"이 "book1"인 도큐먼트 제거
        ```
        > db.books.find({ "name" : "book1"})
        { "_id" : ObjectId("60f636974ac0ab80c78b671c"), "name" : "book1", "author" : "yyj" }
        > db.books.remove({"name" : "book1"})
        WriteResult({ "nRemoved" : 1 })
        > db.books.find()
        { "_id" : ObjectId("60f636174ac0ab80c78b671b"), "name" : "mongodb gide", "author" : "yyj" }
        { "_id" : ObjectId("60f636974ac0ab80c78b671d"), "name" : "book2", "author" : "yyj" }
        ```
# 출처
 - aws 문서 : https://aws.amazon.com/ko/nosql/document/
 - MongoDB 전반적인 내용 : https://velopert.com/436
 - MongoDB 장단점 : https://elky84.github.io/2018/09/26/mongodb/
-  pymongo 라이브러리 : https://www.fun-coding.org/mongodb_basic5.html