用例图

![image-20230626213008056](C:\Users\86173\AppData\Roaming\Typora\typora-user-images\image-20230626213008056.png)



类图

~~~mermaid
classDiagram
    class User {
        <<Entity>>
        -id: int
        -name: string
        -mobileNo: string
        -address: string
        -balance: double
        -password: string
        +register()
        +login()
        +searchWorker()
        +makeAppointment(workerId: int, orderInfo: string)
        +getOrders()
        +pay(orderId: int)
        +cancelOrder(orderId: int)
    }
    class Worker {
        <<Entity>>
        -id: int
        -name: string
        -mobileNo: string
        -address: string
        -rating: double
        -password: string
        +register()
        +login()
        +getOrders()
        +acceptOrder(orderId: int)
        +completeOrder(orderId: int)
    }
    class Admin {
        <<Entity>>
        -id: int
        -name: string
        -mobileNo: string
        -password: string
        +addWorker(workerInfo: string)
        +deleteWorker(workerId: int)
        +editWorker(workerInfo: string)
    }
    class Order {
        <<Entity>>
        -id: int
        -userId: int
        -workerId: int
        -serviceDate: date
        -status: string
        -amount: double
        +create(userId: int, workerId: int, serviceDate: date, orderInfo: string)
        +calculateAmount(serviceDate: date, orderInfo: string)
    }
    User --> Order : 订单
    Worker --> Order : 订单
    Admin --> Worker : 管理员可以对家政工进行管理

    class Comment {
        <<Entity>>
        -id: int
        -userId: int
        -workerId: int
        -createTime: datetime
        -content: text
        +create(userId: int, workerId: int, content: text)
    }
    User --> Comment : 用户可以对家政工进行评价
    Comment --> Worker : 家政工会收到评价信息
    
~~~



```mermaid

sequenceDiagram
    participant User
    participant Platform
    participant OrderSystem
    participant PaymentSystem
    User->>+Platform:搜索工作人员
    Platform->>+OrderSystem:生成订单
    OrderSystem->>-Platform:返回订单号
    Platform->>+PaymentSystem:请求支付
    PaymentSystem->>PaymentSystem:处理支付
    PaymentSystem->>-Platform:返回支付结果
    Platform->>+OrderSystem:更新订单状态
    OrderSystem->>OrderSystem:通知工作人员
    Note over Platform,PaymentSystem: 发送支付成功提示给用户
    Platform-->>-User:支付成功提示
```

