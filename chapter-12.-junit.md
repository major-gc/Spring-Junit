# Chapter 12. 테스트 주도 개발 - TDD

## TDD의 이점

1.리팩토링에 대한 부담감이 줄어든다.
2.TDD를 잘 따른다면 구현하는 실질적인 모든 사례에 대해 단위 테스트를 작성하게 된다.
3.***좋은 코드***로 발전 할 수 있다.

![](.gitbook/assets/tdd.PNG)

1. 실패하는 테스트 코드 작성하기
2. 테스트 통과시키기
3. 이전 두 단계에 추가되거나 변경된 코드 개선하기

## Tdd 같이 해보기

1. Profile 인스턴스 확인

`가. 실패하는 테스트 코드 작성하기`

  ```java
  import org.junit.Before;
  import org.junit.Test;
  
  import static org.junit.Assert.assertFalse;
  
  public class TddProfileTest {
    @Test
    public void ProfileBefore(){
        new TddProfile();
    }
  }
  ```

`나. 테스트 통과시키기`

TddProfile 클래스 추가

  ```java
  public class TddProfile {
    }
  ```


2. Profile이_텅빈경우_매칭이되면안되는_테스트


`가. 실패하는 테스트 코드 작성하기`

  ```java
  import org.junit.Before;
  import org.junit.Test;
  
  import static org.junit.Assert.assertFalse;
  
  public class TddProfileTest {
    @Test
    public void ProfileBefore(){
        new TddProfile();
    }
    
    @Test
    public void Profile이_텅빈경우_매칭이되면안되는_테스트() {
            TddProfile profile = new TddProfile();
            
            TddQuestion question = new TddBooleanQuestion(1,"Relocation package?");
            
            TddAnswer answer = new TddAnswer(question, TddBool.TRUE);
            
            TddCriterion criterion = new TddCriterion(answer,TddWeight.DontCare);
            
            boolean result = profile.mathces(criterion);
            
            assertFalse(result);

    }
    
    @Test
    public void 프로파일에_매칭되는_답변이_포함된경우(){
        TddProfile profile = new TddProfile();
                    
        TddQuestion question = new TddBooleanQuestion(1,"Relocation package?");
        
        TddAnswer answer = new TddAnswer(question, TddBool.TRUE);
        profile.add(answer);
        TddCriterion criterion = new TddCriterion(answer,TddWeight.Important);
    
        boolean result = profile.mathces(criterion);
        //assertFalse(result);
    }
  }
  ```


`나. 테스트 통과시키기`

TddProfile 클래스에 add 할 수 있게

```java
public class TddProfile {
   private TddAnswer answer;

   public boolean matches(TddCriterion criterion) {
      return answer != null;
   }

   public void add(TddAnswer answer) {
      this.answer = answer;
   }
}
```

`다. 이전 두 단계에 추가되거나 변경된 코드 개선하기`

리팩토링 할 것이 있다. 공통부분 리팩토링

```java
public class TddProfileTest {

    TddProfile profile = null;
    TddQuestion question = null;
    TddAnswer answer = null;

    @Before
    public void ProfileBefore(){
        profile = new TddProfile();
    }

    @Before
    public void 질문과답변_설정(){
        question = new TddBooleanQuestion(1,"Relocation package?");

        answer = new TddAnswer(question, TddBool.TRUE);
    }
    
    
    
    .
    .
    .
}
```

3. Profile인스턴스가_매칭되는_Answer객체가없는경우

`가. 실패하는 테스트 코드 작성하기`

```java

import main.ch12.*;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertFalse;

public class TddProfileTest {

    TddProfile profile = null;
    TddQuestion question = null;
    TddAnswer answer = null;

    @Before
    public void ProfileBefore(){
        profile = new TddProfile();
    }

    @Before
    public void 질문과답변_설정(){
        question = new TddBooleanQuestion(1,"Relocation package?");

        answer = new TddAnswer(question, TddBool.TRUE);
    }


    @Test
    public void Profile이_텅빈경우_매칭이되면안되는_테스트() {

       TddCriterion criterion = new TddCriterion(answer,TddWeight.DontCare);

       boolean result = profile.mathces(criterion);

       assertFalse(result);

    }

    @Test
    public void 프로파일에_매칭되는_답변이_포함된경우(){
        profile.add(answer);
        TddCriterion criterion = new TddCriterion(answer,TddWeight.Important);

        boolean result = profile.mathces(criterion);
        //assertFalse(result);
    }

    /*
    테스트가 통과하려면 matches()메서드는 Profile객체가 들고 있는 단일 Answer객체가 Criterion객체에
    저장된 응답과 매칭되는지 정해야 합니다.
     */
    @Test
    public void Profile인스턴스가_매칭되는_Answer객체가없는경우(){
        TddAnswer answerThereIsNot = new TddAnswer(question,TddBool.FALSE);
        profile.add(answerThereIsNot);

        TddCriterion criterion = new TddCriterion(answer,TddWeight.Important);

        boolean result = profile.mathces(criterion);

        assertFalse(result);
    }

}

```

더욱 개선된 최종 코드는 아래의 주소에서 ch12 패키지에 코드가 있습니다.<br> 
> [ch12 최종코드](https://github.com/whdms705/javaAndJunit/tree/master/src)

