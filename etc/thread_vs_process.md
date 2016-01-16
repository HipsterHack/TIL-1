# Thread vs Process

## Process
* Process는 주로 프로그램 인스턴스다.
* 특징은 프로세스  메모리를 다른 메모리 점유한다. 
* 프로세스는 여러 쓰레드를 사용할 수 있다.
* IPC를 이용해서 통신한다. 

## 쓰레드
* Thread는 쓰레드끼리 메모리를 공유한다.
* 그렇기 때문에 synchronization, deadlock 이 발생할 수 있다. 
* little time lag으로 상대적으로 디버깅이 어렵다. 

