---
title: Controller VS RestController
tags: Springboot
---

## Summry  

Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있다. 전통적인 Spring MVC의 컨트롤러인 @Controller와 Restuful 웹서비스의 컨트롤러인 @RestController의 **주요한 차이점은 HTTP Response Body가 생성되는 방식**이다.  

본 문서에서는 @Controller와 @RestController 두 가지 어노테이션의 차이와 사용법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## @Controller(Spring MVC Controller)

### Controller - View

**전통적인 Spring MVC의 컨트롤러인 @Controller는 주로 View를 반환하기 위해 사용한다.** 아래와 같은 과정을 통해 Spring MVC Container는 Client의 요청으로부터 View를 반환한다.  

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. Mapping되는 Handler와 그 Type을 찾는 DispatcherServlet이 요청을 인터셉트한다.
3. Controller가 요청을 처리한 후에 응답을 DispatcherServlet으로 반환하고, DispatcherServlet은 View를 사용자에게 반환한다.

***@Controller가 View를 반환하기 위해서는 ViewResolver가 사용되며, ViewResolver 설정에 맞게 View를 찾아 렌더링한다.***

### Controller - Data

Spring MVC의 컨트롤러에서도 Data를 반환해야 하는 경우도 있다. **Spring MVC의 컨트롤러에서는 데이터를 반환하기 위해 @ResponseBody 어노테이션을 활용해주어야 한다.** 이를 통해 Controller도 Json 형태로 데이터를 반환할 수 있다.  

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. Mapping되는 Handler와 그 Type을 찾는 DispatcherServlet이 요청을 인터셉트한다.
3. @ResponseBody를 사용하여 Client에게 Json 형태로 데이터를 반환한다.

**@RestController가 Data를 반환하기 위해서는 viewResolver 대신에 HttpMessageConverter가 동작한다.** HttpMessageConverter에는 여러 Converter가 등록되어 있고, 반환해야 하는 데이터에 따라 사용되는 Converter가 달라진다. 단순 문자열인 경우에는 StringHttpMessageConverter가 사용되고, 객체인 경우에는 MappingJackson2HttpMessageConverter가 사용되며, 데이터 종류에 따라 서로 다른 MessageConverter가 작동하게 된다. Spring은 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해 적합한 HttpMessageConverter를 선택하여 이를 처리한다.  

### @Controller example code
```java
@Controller
@RequestMapping("/user")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @PostMapping(value = "/info")
    public @ResponseBody User info(@RequestBody User user){
        return userService.retrieveUserInfo(user);
    }
    
    @GetMapping(value = "/infoView")
    public String infoView(Model model, @RequestParam(value = "userName", required = true) String userName){
        User user = userService.retrieveUserInfo(userName);
        model.addAttribute("user", user);
        return "/user/userInfoView";
    }

}
```

위 예제의 info는 User라는 데이터를 반환하고자 하고 있고, User를 json으로 반환하기 위해 @ResponseBody라는 어노테이션을 붙여주고 있다. infoView 함수에서는 View를 전달해주고 있기 때문에 String이 반환값으로 설정되어있다. (이렇게 데이터를 반환하는 RestController와 View를 반환하는 Controller를 분리하여 작성하는 것이 좋다.)

## @RestController(Spring Restful Controller)

@RestController는 Spring MVC Controlle에 @ResponseBody가 추가된 것이다. 당연하게도 **RestController의 주용도는 Json 형태로 객체 데이터를 반환하는 것이다.**  

**동작과정**  
1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. Mapping되는 Handler와 그 Type을 찾는 DispatcherServlet이 요청을 인터셉트한다.
3. RestController는 해당 요청을 처리하고 데이터를 반환한다.

### @RestController example code
```java
@RestController
@RequestMapping("/user")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @PostMapping(value = "/info1")
    public ResponseEntity<User> info1(@RequestBody User user){
        return ResponseEntity.ok(userService.retrieveUserInfo(user));
    }

    @PostMapping(value = "/info2")
    public ResponseEntity<User> info2(@RequestParam(value = "userName", required = true) String userName){
        User user = userService.retrieveUserInfo(userName);

        if(user == null){
            return ResponseEntity.noContent().build()
        }

        return ResponseEntity.ok(user)
    }

    @PostMapping(value = "/info3")
    public ResponseEntity<User> info3(@RequestParam(value = "userName", required = true) String userName){
        return Optional.ofNullable(userService.retrieveUserInfo(userName))
                .map(user -> ResponseEntity.ok(user))
                .orElse(ResponseEntity.noContent().build());
    }
}
```
info1의 메소드는 User 데이터를 그대로 반환하고 있다. 하지만 이렇게 처리하는 것 보다 info2처럼 결과 데이터와 상태코드를 함께 제어하여 반환하는 것이 좋다. 또한 만약 userService에서 반환하는 형태가 Optional이라면 info3 처럼 깔끔하게 처리를 해줄 수 있다.  

## @Controller @RestController 차이

1. @Controller는 클래스를 Spring MVC 컨트롤러로 표시하는데 사용되며, @RestController는 RESTful 웹 서비스에서 사용되는 특수 컨트롤러이며 @Controller + @ResponseBody와 동일히다.
2. @RestController는 Spring4.0에서 추가되었지만, @Controller는 Spring이 주석을 지원하기 시작한 이후에 존재하며 공식적으로 Spring 2.5버전에서 추가되었다.
3. @Controller는 @Component 주석이 달려있고, @RestController는 @Controller와 @ResponseBody 주석이 달린 편의 컨트롤러이다.
4. @Controller와 @RestController의 주요 차이점 중 하나는 **@RestController을 표시하면 모든 메소드가 뷰 대신 객체로 작성된다.**

## Reference

> [[Spring] @Controller와 @RestController 차이 - 망나니개발자](https://mangkyu.tistory.com/49)  
> [@Controller와 @RestController의 차이점 - 기회는 찬스](https://dncjf64.tistory.com/288)  
