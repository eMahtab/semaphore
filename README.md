# Semaphore

### Alternate execution of instance method using Semaphore
```java
import java.util.concurrent.Semaphore;

class Message{
    Semaphore message1Semaphore = new Semaphore(1);
    Semaphore message2Semaphore = new Semaphore(0);
    
    public void message1(){
        try {
			message1Semaphore.acquire();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
        System.out.println(Thread.currentThread().getName()+" is executing message1()");
        message2Semaphore.release();
    }
    public void message2() {
        try {
			message2Semaphore.acquire();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
        System.out.println(Thread.currentThread().getName() + " is executing message2()");
        message1Semaphore.release();
    }
}

public class Main{
    public static void main(String args[]) {
        Message message = new Message();
        Thread t1 = new Thread(() -> message.message1());
        Thread t2 = new Thread(() -> message.message2());
        t2.start();
        t1.start();
        try {
			t1.join();
			t2.join();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
        System.out.println("Main method execution completed !");
    }
}
```
