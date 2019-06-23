---

title: "pandas apply传参"
date: 2019-06-21T11:12:40+08:00
categories:
- python
tags:
- pandas
---
apply函数的*args和**kwds参数， 

```python
import pandas as pd
import datetime   #用来计算日期差的包
def dataInterval(data1,data2):
    d1 = datetime.datetime.strptime(data1, '%Y-%m-%d')
    d2 = datetime.datetime.strptime(data2, '%Y-%m-%d')
    delta = d1 - d2
    return delta.days
def getInterval_new(arrLike,before,after):  #用来计算日期间隔天数的调用的函数
    before = arrLike[before]
    after = arrLike[after]
#    print(PublishedTime.strip(),ReceivedTime.strip())
    days = dataInterval(after.strip(),before.strip())  #注意去掉两端空白
    return days

if __name__ == '__main__':    
    fileName = "NS_new.xls";
    df = pd.read_excel(fileName) 
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, args = ('ReceivedTime','PublishedTime'))    #调用方式一
    #下面的调用方式等价于上面的调用方式
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, **{'before':'ReceivedTime','after':'PublishedTime'})  #调用方式二
    #下面的调用方式等价于上面的调用方式
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, before='ReceivedTime',after='PublishedTime')  #调用方式三

```



<!--more-->