# @Controller와 @RestController의 차이

- ```@RestController```는 ```@Controller```에 ```@ResponseBody```가 추가된 것

예전에 프로그래밍을 할 때에는 jsp나 html과 같은 View를 반환했기 때문에 ```@Controller```를 사용해왔었지만, 

최근에는 프론트엔드와 백엔드를 따로 두고, 백엔드에서는 View가 아닌 REST API를 통한 json 데이터만 전달하는 추세라서
```@RestController```가 등장하게 된 것이다.

**데이터를 반환하는** ```@RestController```와 **View를 반환하는** ```@Controller```는 분리하여 작성하는 것이 좋다.

# Reference

https://mangkyu.tistory.com/49