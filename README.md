#缓存架构
##一.nginx分发层
* 1.动态负载均衡(地址从consul中获取)
* 2.动态切换负载均衡的方式(从consul获取)
##二.nginx应用层
* 1.nginx内存缓存,使用mlcache(字典)缓存热点数据,缓存会过期
* 2.利用布隆过滤器防止缓存穿透(过滤异常请求)
* 3.从redis集群中尝试读取缓存,读取到把值设置到nginx字典中(分布式锁,防止缓存击穿)
* 4.经过去重队列后,尝试从源服务器当中读取(防止脏数据,旧数据覆盖新数据的缓存)
* 5.lua做模板动态渲染(前后端分离可以直接返回数据)
* 6.缓存失效时间随机(防止缓存雪崩)
* 7.限流,降级
