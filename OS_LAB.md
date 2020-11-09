# OS_LAB

## A.复旦早餐王

### QUESTION 1

 互斥：一个公共资源不能被多个程序使用，由于早餐店是公共资源，两个**队伍**不能同时使用早餐店。

 同步：由于早餐店只存在一个篮子，所以煎饼果子与鸡蛋灌饼应叫交替运行。只有老板或者老板娘将物品放入篮子中，两只对于才可以拿走货物。

## QUESTION 2

```c
var items = 0 ,window_size = 1 ,mutex =1;
//如果缓冲区为1则不需要互斥信号量
var Chinese hamburger= 0,Eggs filled pie = 0;
//煎饼果子，鸡蛋灌饼
producer1()
{
    while(1)
    {
        p(Chinese hamburger);
        p(mutex);
        /*煎饼果子放入篮子，临界区*/
        v(mutex);
        v(Chinese hamburger);
    }
}
```

```c
producer2()
{
	while(1)
    {
        p(Eggs filled pie);
        p(mutex);
        /*鸡蛋灌饼放入篮子，临界区*/
        v(mutex);
        v(Eggs filled pie);
    }
}
```

```c
customer1()
{
	while(1)
    {
        p(Eggs filled pie);
        p(mutex);
        /*鸡蛋灌饼拿走，临界区*/
        v(mutex);
        v(Eggs filled pie);
    }
}
```

```c
customer2()
{
	while(1)
    {
        p(Chinese hamburger);
        p(mutex);
        /*煎饼果子拿走，临界区*/
        v(mutex);
        v(Chinese hamburger);
    }
}
```

## B.小小打印店问题

### QUESTION 1

互斥：题目中的公共资源为打印机，即打印机被所有顾客进程所共享，所以设置互斥量来实现互斥。

同步：由于理发师只有一个，所以只有在当前顾客将打印机置为睡眠状态，下一个顾客唤醒打印机才能使用。

### QUESTION 2

```c
var waiting = 0 ;
var waiting_area;
semaphore customers = 0 , printer = 0，mutex =1;

void printer()
{
    while(1){
        p(customers);//wait+1
        p(mutex);
        //进入临界区
        waiting--;
        v(printer);
        v(mutex);
        print();
    }
}
void customer()
{
	p(mutex);//进入临界区
    if(waiting < waiting area){
        waiting++;
        v(customers);//唤醒打印机
        v(mutex);//退出临界区
        p(printer);//顾客等待
        get_wait_area();
    }else{
        v(mutex);//直接离开
    }
}
```

