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

## c.哲学家就餐问题

使用AND信号量机制解决死锁问题，末尾哲学家进入临界区之后，才将筷子分配给他，然后进餐，否则不给他资源。使用全局互斥量mutex来限制全部的临界区的资源，当有哲学家拿起筷子的时候，将所有的资源锁住，放下筷子的时候释放资源。

mutex的初始化

在.h中

```c
pthread_mutex_t mutex[1];
```

main.c中

```c
pthread_mutex_init(&mutex[0], NULL);
```

philosopher.c中

```c
void *philosopher(void *philosopherNumber) {
	int i = (int)philosopherNumber;//int i = *(int *)arg;
	//int left = i;
	//int right = (i+1)%NUMBER_OF_PHILOSOPHERS;
	while (1) {
		/*Your code here*/
		//printf("unimplemented yet\n");
		think(i);//思考
		pthread_mutex_lock(&mutex[0]);//进入互斥
		pickUp(i);//拿起筷子
		pthread_mutex_unlock(&mutex[0]);//释放互斥
		eat(i);//吃
		putDown(i);//放下筷子
		//exit(1);
	}
}
```

伪代码

```c
void philosopher (void* arg) {
    while (1) {
        think();
        hungry();
        pthread_mutex_lock(&chopsticks[left]);
        pthread_mutex_lock(&chopsticks[right]);
        eat();
        pthread_mutex_unlock(&chopsticks[left]);    
        pthread_mutex_unlock(&chopsticks[right]);
    }
}
```

在实现对于筷子的互斥时，筷子的初始化为

```c
	for (i = 0; i < NUMBER_OF_PHILOSOPHERS; ++i) {
		pthread_mutex_init(&chopsticks[i], NULL);
	}
```

所以在拿起和放下筷子的时候的信号量为同意对信号量：

```c
void pickUp(int philosopherNumber) {
	// request chopsticks
    /*Your code here*/
	int left = philosopherNumber;//左筷子为本身
    int right = (philosopherNumber + 1) % 5;//有筷子为+1取模
	pthread_mutex_lock(&chopsticks[right]);//锁住筷子
	pthread_mutex_lock(&chopsticks[left]);
	printf("Philosopher %d pick up %d and %d chopsticks\n", philosopherNumber,left,right);
}
```

````c
void putDown(int philosopherNumber) {
	// release chopsticks
    /*Your code here*/
	int left = philosopherNumber;
    int right = (philosopherNumber + 1) % 5;
	pthread_mutex_unlock(&chopsticks[right]);//释放筷子
	pthread_mutex_unlock(&chopsticks[left]);
	printf("Philosopher %d pick down %d and %d chopsticks\n", philosopherNumber,left,right);
}
````

linux的运行截图。

![](C:\Users\0x0\Downloads\uTools_1604931254104.png)

![](C:\Users\0x0\Downloads\uTools_1604934360109.png)