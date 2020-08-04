---
title: JAVA for each(for문 향상버전)
tags: JAVA, Android
---

JAVA에서 뿐만 아니라 다양한 언어에서 for문은 굉장히 많이 사용되는 반복문이다.  
반복적인 기능을 수행하기 때문에 배열등과 함께 쓰이는 경우가 빈번하다.  

JDK 5.0 version부터는 for each문을 지원하기 때문에 좀 더 편하게 for문에서 배열등을 사용할 수 있다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

**for문의 기본 문법**  
for(초기식;조건식;증감식){  
    반복기능수행;  
}  

{% highlight JAVA linenos %}  
public class test{
    public static void main(String[] args){
        int ary[] = {1,2,3,4,5};

        for(int i = 0; i < ary.length; i++){
            System.out.println(ary[i]);
        }
    }
}

{% endhighlight %}  

**for each문법**  
for(타입 변수명;배열이름){  
    반복기능수행;  
}  

{% highlight JAVA linenos %}  
public class test{
    public static void main(String[] args){
        int ary[] = {1,2,3,4,5};

        for(int i : array){
            System.out.println(i);
        }
    }
}

{% endhighlight %}  

for each에서는 배열의 index만큼 반복하며 매 반복마다 배열의 원소를 차례로 꺼내어 변수(i)에 대입한다.  

**위의 두 소스는 동일한 결과를 출력한다**  

for each에서는 배열의 값을 **수정할 수 없고** 값을 읽어 **사용할 수만 있다**.  
