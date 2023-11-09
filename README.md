> QuanX configutation

### [general] 部分
- 资源解析器，可用于自定义各类远程资源（服务器订阅、rewrite、filter）的转换

  resource_parser_url= [https://fastly.jsdelivr.net/gh/KOP-XIAO/QuantumultX@master/Scripts/resource-parser.js](https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/resource-parser.js)

### [policy] 部分
- static 策略组

  手动选择节点/策略组

- available 策略组

  按顺序选择列表中第一个可用的节点

- round-robin 策略组

  按列表的顺序轮流使用其中的节点

- url-latency-benchmark 延迟策略组

  选取延迟最优节点

- dest-hash 策略组

  随机负载均衡，但相同域名走固定节点

- ssid 策略组

  根据你所设定的网络来自动切换节点/策略组
