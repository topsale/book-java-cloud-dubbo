# RabbitMQ WebUI

---

## 访问地址

http://ip:15672

## 首页

![](/assets/Lusifer2018050122030001.png)

### Global counts

![](/assets/Lusifer2018050122030002.png)

- Connections：连接数
- Channels：频道数
- Exchanges：交换机数
- Queues：队列数
- Consumers：消费者数

## 交换机页面

![](/assets/Lusifer2018050122030003.png)

## 队列页面

![](/assets/Lusifer2018050122030004.png)

- Name：消息队列的名称，这里是通过程序创建的
- Features：消息队列的类型，durable:true为会持久化消息
- Ready：准备好的消息
- Unacked：未确认的消息
- Total：全部消息
- 备注：如果都为 0 则说明全部消息处理完成
