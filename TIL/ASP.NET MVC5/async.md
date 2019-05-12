# IIS에서 ASP.NET MVC의 쓰레드 및 동기, 비동기와 병렬처리

IIS(웹서버)에서 ASP.NET의 요청을 처리하는데 사용되는 Thread Pool이 있는데, Thread Pool에는 Thread의 수가 제한되어 있습니다.

Request가 들어오면 Thread Pool에서 관리하고 있던 한 개의 Thread를 꺼내서 해당 Request를 처리합니다.
그런데 요청에 대한 처리를 동기 작업으로 한다면 Thread는 요청이 완료될 때까지 기다려야 합니다.
동시적으로 Request가 많으면 결국에는 이 모든 Thread가 사용 중이 되고,
그 이후에 또 요청하게 되면 HTTP 503(서버 사용량이 많음)을 반환하며 요청을 거부합니다.

처음에 저도 착각을 했던 것이 동기 작업으로 10초가 걸리는 작업을 비동기로 하면 시간이 더 단축된다라고 생각했습니다.
하지만 비동기로 변경을 한다는 것은 Thread를 더 효율적으로 사용하는 것이며 동시적으로 Request가 많을 때, 이점을 보는 것입니다.

조금 간단히 설명해드리면 전화기로 중국집에 자장면 배달을 시킨 이후에 다른 작업을 하지 않고 자장면이 오면 먹은 이후에 집안일을 하는 것은 동기식입니다.
반면, 자장면 배달을 시킨 이후에 빨래도 하고 청소도 하면서 집안일을 하고 자장면이 배달이 오면 먹는 것이 비동기식입니다.
.NET의 웹서버에서도 Thread가 비동기 호출이 진행되는 동안은 Thread Pool로 반환됩니다. Thread가 Thread Pool에 있는 동안 더는 이 해당 Request와는 관련이 없습니다.

https://msdn.microsoft.com/en-us/magazine/dn802603.aspx 에 그림과 함께 설명이 되어 있습니다.

여러 비동기 작업을 병렬로 처리하여 속도를 빠르게 하고 싶으면 Task.WhenAll을 사용해야 합니다.

var task1 = DoWorkAsync();
var task2 = DoMoreWorkAsync();

await Task.WhenAll(task1, task2);
Task.WaitAll은 모든 것이 완료될 때까지 현재 스레드를 차단하는 것이며, (동기)
Task.WhenAll은 모든 것이 완료될 때까지 기다리는 동작을 나타내는 작업을 반환합니다. (비동기)

var task1 = await DoWorkAsync();
var task2 = await DoMoreWorkAsync();
위 코드는 두 작업을 병렬로 동시에 실행하지 않습니다. DoWorkAsync()를 실행하고, 다음에 DoMoreWorkAsync()를 실행합니다.
하지만 비동기 코드이기 때문에 Thread에서 이점이 있습니다.

---
#### 참고

https://msdn.microsoft.com/en-us/magazine/dn802603.aspx

https://nsinc.tistory.com/110

https://docs.microsoft.com/ko-kr/aspnet/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
