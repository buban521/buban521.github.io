---
layout: post
title: "调用天气查询webservice服务的方法"
tag: 工具 
---
## 调用天气查询webservice服务的方法
- 1.生成xml文件

在在浏览器地址栏输入http://ws.webxml.com.cn/WebServices/WeatherWS.asmx?WSDL,把页面保存到磁盘.我保存在D:/WeatherWebService.asmx.xml,名字有可能不一样,换成你自己的名字就可以

- 2.在命令提示符窗口内输入wsimport -extension -s .  D:\WeatherWebService.asmx.xml

这里解释一下:
wsimport是jdk的命令,如果提示没有这个命令,要先配好Java环境,-s是生成.java文件, (.)表示在当前目录生成,后面是上面保存的文件.
如果提示错误,无法生成文件,把文件里与错误提示行对应的行删除,然后再运行上面的命令,会在当前目录生成文件.

- 3.在Eclipse中新建Java工程,把上面生成的文件拷贝到工程中

- 4.新建一个测试类

- 5.下面为调用webservice服务的方法
由于已经不能免费调用了,下面的方法为调用该平台支持某省份下哪些城市.

```
package cn.lszz;

import java.util.List;

import cn.com.webxml.ArrayOfString;
import cn.com.webxml.WeatherWS;
import cn.com.webxml.WeatherWSSoap;

public class testClient {

	public static void main(String[] args) {
		// 创建服务视图
				WeatherWS weatherWS = new WeatherWS();
				// 通过服务视图得到服务端点
				WeatherWSSoap weatherWSSoap = weatherWS.getWeatherWSSoap();
				// 调用webservice服务方法
				ArrayOfString arrayOfString = weatherWSSoap.getSupportCityString("浙江");
				List<String> resultlist = arrayOfString.getString();

				for (String result : resultlist) {
					System.out.println(result);
				}
	}
}
```

