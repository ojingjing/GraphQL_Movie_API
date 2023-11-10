# GraphQL로 영화 API 만들기 🎥

---
node.js , GraahQL , HASURA (내 db에 즉각적으로 graph api를 만들어준다)
---

## GraphQL이란 ?
API이란 쿼리 언어이며 클라에서 필요한것을 정확하게 요청할수 있는 기술이다.

### GraphQL vs REST API
공통점 : 벡엔드와의 의사소통을 위한것

REST api 는 GET,POST,PUT,DELETE 
URL 을 이용하여 사용할수 있다.

GraphQL은 RESTapi의 단점을 보완한 것이다.

### RESTAPI의 단점    
1.over-fetching    
쓰지않는 데이터임에도 모든 데이터를 불러오는 것    
=> DB가 일을 더 많이 하기 때문에 속도 저하    

2.under-fetching     
우리가 필요한것보다 데이터를 덜 받는것     
json 내 정보가 제대로 나와있지 않아 다른 url 을 한번더 requset 해야하기 때문에 requset를 두번해야하는 번거로움이 있고 로딩시간이 길어질수 있다는 점과 두개중 하나는 requset를 실패할수도 있다는 단점이 있다    

### GraphQL이 보완한 점
1. 쓰지않는 데이터를 모두 불러오는게 아닌 필요한 data 만 요청할수 있다.
2. 모든것이 type을 가지고 있다.


ex )   
```
{
  "links": {
    "self": "http://example.com/articles",
    "next": "http://example.com/articles?page[offset]=2",
    "last": "http://example.com/articles?page[offset]=10"
  },
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON:API paints my bikeshed!"
    },
    "relationships": {
      "author": {
        "links": {
          "self": "http://example.com/articles/1/relationships/author",
          "related": "http://example.com/articles/1/author"
        },
        "data": { "type": "people", "id": "9" }
      },
      "comments": {
        "links": {
          "self": "http://example.com/articles/1/relationships/comments",
          "related": "http://example.com/articles/1/comments"
        },
        "data": [
          { "type": "comments", "id": "5" },
          { "type": "comments", "id": "12" }
        ]
      }
    },
    "links": {
      "self": "http://example.com/articles/1"
    }
  }],
  "included": [{
    "type": "people",
    "id": "9",
    "attributes": {
      "firstName": "Dan",
      "lastName": "Gebhardt",
      "twitter": "dgeb"
    },
    "links": {
      "self": "http://example.com/people/9"
    }
  }, {
    "type": "comments",
    "id": "5",
    "attributes": {
      "body": "First!"
    },
    "relationships": {
      "author": {
        "data": { "type": "people", "id": "2" }
      }
    },
    "links": {
      "self": "http://example.com/comments/5"
    }
  }, {
    "type": "comments",
    "id": "12",
    "attributes": {
      "body": "I like XML better"
    },
    "relationships": {
      "author": {
        "data": { "type": "people", "id": "9" }
      }
    },
    "links": {
      "self": "http://example.com/comments/12"
    }
  }]
}
```
## SETTING🔧

package.json
"type" :"module" 을 추가해주게 되면 import 문을 쓸수 있기 때문이다.
```
import { ApolloServer , gql } from "apollo-server";

const {ApolloServer,gql} = require("apollo-server")

```
```
$ npm i nodemon -D 
```
파일을 저장할때마다 nodemon 이 서버를 재시작 시키는 역할을 해준다. 나은 개발을 위한 것   

```
$ npm run dev
```
로 실행해서 성공하게 되면   
![image](https://github.com/ojingjing/GraphQL_Movie_API/assets/48702158/0e79665f-e0d0-4cbf-960a-9edd8d7b6864)

![image](https://github.com/ojingjing/GraphQL_Movie_API/assets/48702158/587573ce-6c6a-4af5-959e-753f51489707)

![image](https://github.com/ojingjing/GraphQL_Movie_API/assets/48702158/03112ba5-8058-4979-9549-5db977a076db)


## CODE💡

```
const typeDefs = gql``    
```
여기에다 type 들을 지정해준다.
```
#GET /text
#GET /hello
#POST,PUT,DELETE
const typeDefs = gql`
    type Query {
        text: String
        hello:String
    }
	type Mutation{
	    postTweet(text:String , userId :ID) :Tweet  # authentication
	  }
`;

```
GraphAL의 Query는 REST api 에서 GET으로 받아는 것과 동일한 뜻이라고 생각하면 된다. ⇒사용자가 requset 하고싶어하는것을 선언하면 된다.
```
const resolvers = {    
  Query: {
    tweet() {
      console.log("I'm called");
      return null;
    },
  },
};
const server = new ApolloServer({ typeDefs, resolvers });
```
resolver은 사용자가 호출할때 쓰는것이다.

---
[노션주소]https://creative-respect-76a.notion.site/ecole-5Day-67ca5266c2524ccaab16fca90b12491b?pvs=4
