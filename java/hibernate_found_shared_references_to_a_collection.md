# HibernateException - Found shared references to a collection #

在使用`BeanUtils.copyProperties(source, target)`拷貝物件時
hibernate經常會告知`Found shared references to a collection`
原因就是因為在拷貝OneToMany或ManyToOne時，會用到下列的方式
```java
newEntity.setXxxCollection(oldEntity.getXxxCollection())
```
由於`BeanUtils.copyProperties(source, target)`是淺拷貝
所以copy完後新舊兩者參考到同一份collection

解決方式如下
* 1. 使用 `newEntity.setXxxCollection(null)`
* 2. 逐一每個欄位進行拷貝。但將來新增欄位時，容易遺漏。

參考：http://jeanne-bug.blogspot.com/2009/07/hibernate-found-shared-references-to.html