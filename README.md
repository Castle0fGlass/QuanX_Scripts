> QuanX configutation

### [general] 部分
- 资源解析器，可用于自定义各类远程资源（服务器订阅、rewrite、filter）的转换

  ```
  resource_parser_url = https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/resource-parser.js
  ```

- running_mode_trigger 即根据网络自动切换 分流/直连/全局代理 等模式，跟手动切换直连/全局代理 等效，rewrite/task 模块始终会生效，比 ssid 策略组设置简单，比 ssid-suspend 更灵活

  ```
  running_mode_trigger=filter, filter, [WIFI SSID1]:all_direct, [WIFI SSID2]:all_direct
  ```
  
  上述写法，前两个 filter 先后表示 在 [数据蜂窝网络] 跟 [一般 Wi-Fi] 下，走 filter(分流)模式，后面则表示在 WIFI SSID1 下切换为全局直连[all_direct]，WIFI SSID2 下切换为全局代理[all_proxy]，相应 SSID 换成自己 Wi-Fi 名即可

- ssid_suspended_list 让 Quantumult X 在特定 Wi-Fi 网络下暂停工作(仅 task 模块会继续工作)，多个Wi-Fi用“,”连接

  ```
  ssid_suspended_list=[WIFI SSID1], [WIFI SSID2]
  ```

- server_check_url

  对指定的网址进行相应测试，以确认节点的可用性

- server_check_timeout

  节点延迟测试超时参数，超出则节点不可用

- geo_location_checker

  用于节点页面的节点信息展示，可自定义展示内容与方式

- dns exclusion list 中的域名将不使用fake-ip方式. 其它域名则全部采用 fake-ip 及远程解析的模式

- udp_whitelist UDP 端口白名单，留空则默认所有为端口。不在udp白名单列表中的端口，将被丢弃处理（返回 ICMP  “端口不可达” 信息）

- udp_drop_list 同白名单类似，但不会返回 ICMP “端口不可达” 信息

### [dns] 部分
- no-system、no-ipv6 禁用系统 DNS（no-system） 以及 ipv6

- server 指定 dns 服务器，并发响应选取最优结果

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

### [filter] 部分
[policy]（策略组） 是为 [filter]（分流）服务的
如开启其他设置中的  “分流匹配优化” 选项，则匹配优先级为 host > host-suffix > host-keyword(wildcard) > geoip = ip-cidr > user-agent
- host 完整域名匹配

- host-keyword 域名关键词匹配

- host-suffix 域名后缀匹配

- host-wildcard 域名通配符匹配

- user-agent 匹配

```
//强制分流走蜂窝网络
;host-suffix, googleapis.com, proxy, force-cellular
//让分流走蜂窝网络跟 Wi-Fi 中的优选结果
;host-suffix, googleapis.com, proxy, multi-interface
//让分流走蜂窝网络跟 Wi-Fi 中的负载均衡，提供更大带宽出入接口
;host-suffix, googleapis.com, proxy, multi-interface-balance
//指定分流走特定网络接口
;host-suffix, googleapis.com, proxy, via-interface=pdp_ip0

// %TUN% 参数，回传给 Quantumult X 接口，可用于曲线实现代理链功能
;host-suffix, example.com, ServerA, via-interface=%TUN%
;ip-cidr, ServerA's IP Range, ServerB
```

### [task_local] 部分
包含 3 种类型: cron 定时任务，UI交互脚本，网络切换脚本
- cron 定时任务

  从 “分” 开始的5位cron 写法

- event-interaction UI交互脚本

- event-network 网络切换/变化时 触发的脚本

### [mitm] 部分
- passphrase、p12 证书参数，可去UI界面自行生成并安装证书，会在此生成对应信息

- hostname 主机名

- skip_src_ip 当使用 Quantumult X 在 M 芯片的 Mac 设备上作为局域网网关时，使用下面的参数来 跳过某些特定设备的 mitm 需求

