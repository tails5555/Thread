﻿# Thread 이해한 것들	

 - **Thread는 하나의 실행 흐름이다.**

Thread는 코드를 읽어 그걸 실행하는 실행 흐름이다. 

실행 흐름을 알기 전에 평소에 우리가 작성하는 코드의 실행과정을 확인해보자

코드를 작성하고 컴파일을 한 뒤 프로그램을 실행시킨다. 그러면 프로세스가 메모리에 올라가게 된다. 여기까지만 하면 프로그램은 실행되지 않는다. 우리가 평소에 일하는 작업이랑 똑같다. 누군가가 일을 하기 위한 자료나 도구를 책상 위에 올려놓는다고 해서 작업은 시작되지 않는다. 누군가가 그 작업도구로 일을 진행해야만 작업이 이루어진다.  그렇다면 우리가 그 작업자를 지정해주지 않았는데 어떻게 메모리에 올라간 코드들이 실행되는 것일까? 프로세스가 메모리에 올라간 뒤 main Thread 작업자가 그 코드를 실행하기 때문이다. 우리가 명시적으로 Thread 를 만들어주지 않았지만 main Thread는 자동으로 생성되어 진행된다.

 1. 프로세스가 메모리에 올라간다.
 2. main Thread 흐름이 생성된다.
 3. main Thread가 main 메소드를 실행한다.

이와 같이 우리가 평소에 생각하지 않아도 쓰레드는 기본적으로 작동하고 있다.

## 동기와 비동기

 - 동기

어떠한 작업을 한 쓰레드가 담당하여 처리하는 것이 동기이다. 우리가 평소에 쓰레드를 새로 생성하지 않고 작업하는 것이 동기라고 생각하면 된다. main 쓰레드 하나만이 코드를 실행하고 있기 때문이다. 


>나는 동기란 순차적으로 진행하다는 의미로 이해했다. 어떠한 코드를 한 순간에는 하나의 스레드만 읽고 실행한다는 의미이다.

 - 비동기

어떠한 작업을 한 쓰레드가 담당하는 것이 아니라 여러 쓰레드가 동시에 진행하는 것이 비동기이다. 1 부터 1,000까지 혼자서 더하지 말고 1 부터 100, 200 부터 300 ... 900 부터 1,000 까지 10 명의 사람을 동원하여 더하는 것이 비동기적 처리이다. 어떤 작업을 여러 쓰레드가 담당하여 처리하는 것이다.

그런데 여떤 작업을 여러 쓰레드가 담당하면 문제가 생길 수 있다. 어떠한 값을 동시에 수정할 수 있기 때문이다. 이런 것을 방지하기 위해 동기화를 해줘야 한다.

> 하나의 값을 여럿이서 수정하는 문제 외에도 쓰레드가 많으면 다른 여러 문제들이 생길 수 있다. 쓰레드 하나당 메모리가 배정되어야 하기 때문에 메모리 누수 문제가 생길 수 있고 cpu에서 여러 쓰레드를 처리하기 위해 context switching 이 발생하는데 너무 많을 경우 context switching 이 불필요하게 발생하거나 자기 차례가 너무 나중에 돌아와 전체 실행이 느려질 수 있기 때문이다.

 - 동기화

여러 쓰레드가 하나의 메소드를 동시에 호출한다고 가정하자 그런데 그 메소드는 어떠한 값을 바꾸는 메소드였다. 그러면 우리는 그 메소드에 **synchronized** 를 붙여줘 하나의 쓰레드만 접근할 수 있게 만든다. 이렇게 어떠한 작업을 하나의 실행흐름만이 작업하게 하는 것을 동기화라고 한다. 

이렇게 하나의 쓰레드만이 그 메소드를 실행할 수 있게 해주는 것이synchronized인데 그러면 synchronized 는 어떻게 작동할까?

## synchronized
synchronized 는 자바에서 동기화를 지원해주는 키워드이다. synchronized 키워드가 붙어 있는 블럭이나 메소드는 쓰레드가 여럿 들어와도 동기적으로 진행되게 해준다. 내부적으로 진행되는 것을 말하기 전에 간단한 예제를 말해보자.

우리가 공중 화장실을 사용하려고 한다. 공중 화장실이기 때문에 아무나 사용할 수 있다. 하지만 한 순간에 사용할 수 있는 사람은 한 명 뿐이다. 그렇기 때문에 우리는 화장실 문을 잠그고 들어간다. 화장실 문을 잠그면 다른 사람은 들어올 수 없기 때문이다. 그리고 뒤에 사람들은 순서대로 줄을 서서 앞에 사람이 끝나고 잠금을 풀어주기를 기다린다.

위의 예제처럼 화장실 문을 잠그고 줄을 세우고 잠금을 풀어주는 것이 synchronized 키워드가 해주는 일이다. 어떤 쓰레드가 synchronized 키워드가 붙은 구절에 들어가려고 하면 먼저 잠겼는지 확인한다. 잠겼으면 그 쓰레드 흐름을 모니터 대기 큐에 넣는다. 한 마디로 순서대로 들어가게 줄을 세우는 것이다. 그 뒤 앞에 사람이 일을 끝냈으면 잠금을 풀고 다음 사람을 넣어주고 잠근다. 이러한 과정을 프로그래머가 작성하지 않아도 synchronized 키워드만 적어주면 작동한다.

> 모니터 대기 큐라는 것을 알기 위해서는 lock 을 무엇을 기준으로 거는지 알아야 한다. synchronized는 객체를 기준으로 lock을 건다. 그 객체의 ~~~ 에 접근하려면 lock이 걸렸는지 확인하고 그 객체에 접근하려고 했으니 그 객체의 대기 큐에 넣어서 앞에 쓰레드가 일이 끝나면 대기 큐에 들어 있는 쓰레드를 꺼내서 실행시킨다.

## 동기화의 문제

위에 synchronized를 통해서 lock을 사용하여 동기화를 하는 것을 알았다. 하지만 이는 프로그램이 결과를 내는 데에 있어 크리티컬한 문제는 아니지만 성능면에서는 크리티컬한 문제이다. lock을 거는 것이 생각 외로 많은 시간을 잡아먹기 때문이다. lock을 하나 거는 것쯤은 문제가 없다. 하지만 대량의 쓰레드가 하나의 객체에 접근한다고 하면 이 lock을 거는 것은 정말로 큰 문제이다. 그렇기 때문에 어떨 때 동기화를 하고 어떨 때 비동기를 할지 잘 생각해야한다. 그러기 위한 concurrency pattern 들이 있다. 이 레퍼지토리는 사실 이걸 위한 레퍼지토리이다.

