# ruleset
- 从本地或 url 获取规则片段，格式为 Group name,[type:]URL[,interval] 或 Group name,[]Rule
- 支持的type（类型）包括：surge, quanx, clash-domain, clash-ipcidr, clash-classic，type留空时默认为surge类型的规则
- [] 前缀后的文字将被当作规则，而不是链接或路径，主要包含 []GEOIP 和 []MATCH(等同于 []FINAL)
```
ruleset=🍎 苹果服务,https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Apple.list
# 表示引用 https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Apple.list 规则
# 且将此规则指向 [proxy_group] 所设置 🍎 苹果服务 策略组
ruleset=Domestic Services,clash-domain:https://ruleset.dev/clash_domestic_services_domains,86400
# 表示引用clash-domain类型的 https://ruleset.dev/clash_domestic_services_domains 规则
# 规则更新间隔为86400秒
# 且将此规则指向 [proxy_group] 所设置 Domestic Services 策略组
ruleset=🎯 全球直连,rules/NobyDa/Surge/Download.list
# 表示引用本地 rules/NobyDa/Surge/Download.list 规则
# 且将此规则指向 [proxy_group] 所设置 🎯 全球直连 策略组
ruleset=🎯 全球直连,[]GEOIP,CN
# 表示引用 GEOIP 中关于中国的所有 IP
# 且将此规则指向 [proxy_group] 所设置 🎯 全球直连 策略组
ruleset=!!import:snippets/rulesets.txt
# 表示引用本地的snippets/rulesets.txt规则
```
# custom_proxy_group
- 为 Clash 、Mellow 、Surge 以及 Surfboard 等程序创建策略组, 可用正则来筛选节点
- [] 前缀后的文字将被当作引用策略组
```
custom_proxy_group=Group_Name`url-test|fallback|load-balance`Rule_1`Rule_2`...`test_url`interval[,timeout][,tolerance]
custom_proxy_group=Group_Name`select`Rule_1`Rule_2`...
# 格式示例
custom_proxy_group=🍎 苹果服务`url-test`(美国|US)`http://www.gstatic.com/generate_204`300,5,100
# 表示创建一个叫 🍎 苹果服务 的 url-test 策略组,并向其中添加名字含'美国','US'的节点，每隔300秒测试一次，测速超时为5s，切换节点的延迟容差为100ms
custom_proxy_group=🇯🇵 日本延迟最低`url-test`(日|JP)`http://www.gstatic.com/generate_204`300,5
# 表示创建一个叫 🇯🇵 日本延迟最低 的 url-test 策略组,并向其中添加名字含'日','JP'的节点，每隔300秒测试一次，测速超时为5s
custom_proxy_group=负载均衡`load-balance`.*`http://www.gstatic.com/generate_204`300,,100
# 表示创建一个叫 负载均衡 的 load-balance 策略组,并向其中添加所有的节点，每隔300秒测试一次，切换节点的延迟容差为100ms
custom_proxy_group=🇯🇵 JP`select`沪日`日本`[]🇯🇵 日本延迟最低
# 表示创建一个叫 🇯🇵 JP 的 select 策略组,并向其中**依次**添加名字含'沪日','日本'的节点，以及引用上述所创建的 🇯🇵 日本延迟最低 策略组
custom_proxy_group=节点选择`select`(^(?!.*(美国|日本)).*)
# 表示创建一个叫 节点选择 的 select 策略组,并向其中**依次**添加名字不包含'美国'或'日本'的节点
```
