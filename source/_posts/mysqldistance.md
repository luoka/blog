title: "MySQL-通过经纬度计算两点间的距离"
date: 2015-04-08 15:33:59
tags:
- MySQL
- 技术
categories: 技术
---

## 计算公式，单位米
第一个点的经纬度(lng1, lat1)
第二个点的经纬度(lng2, lat2)

公式：

```mysql
round(6378.138*2*asin(sqrt(pow(sin( (lat1*pi()/180-lat2*pi()/180)/2),2)+cos(lat1*pi()/180)*cos(lat2*pi()/180)* pow(sin( (lng1*pi()/180-lng2*pi()/180)/2),2)))*1000)
```

{% blockquote 参考博文 http://blog.sina.com.cn/s/blog_8da982ac0101jeh5.html 根据两点经纬度计算距离 %}
{% endblockquote %}