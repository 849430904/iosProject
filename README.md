* OC项目

### LBStoreMall 电商，只弄了首页
[地址](https://github.com/lb2281075105/LBStoreMall)

----
### 直播 https://github.com/ChinaArJun/Tencent-NOW 
---
*  
* Swift项目
----
### Swift-BanTang-master

---

###swift3.0高仿微信 https://github.com/shaoyanglichao01/LXFWeChat

----

	* '++' is deprecated: it will be removed in Swift 3
	
	```
	for var i = 0 ; i < data?.count; i++
	改为：
	for var i in 0..data.count 
	``` 
	* NSBundle.mainBundle.pathForResource
	
	```
	
	let path = NSBundle.mainBundle().pathForResource("tutorials", ofType: nil)
	修改为
	 let path = Bundle.main.url(forResource:"tutorials", withExtension: "json") 	
	
	``` 
* 常用第三方库