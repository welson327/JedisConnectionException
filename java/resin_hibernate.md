## NoSuchMethodError : debugf ##

在Resin 4.0 搭配Hibernate 5.0.12時遇到以下Exception
```
2018-11-05 11:06:40 [JdbcEnvironmentInitiator.java:72] Database ->
       name : MySQL
    version : 5.6.26-log
      major : 5
      minor : 6
2018-11-05 11:06:40 [JdbcEnvironmentInitiator.java:83] Driver ->
       name : MySQL Connector/J
    version : mysql-connector-java-8.0.12 (Revision: 24766725dc6e017025532146d94c6e6c488fb8f1)
      major : 8
      minor : 0
[18-11-05 11:06:40.766] {resin-port-8080-26} java.lang.NoSuchMethodError: org.hibernate.internal.CoreMessageLogger.debugf(Ljava/lang/String;II)V
                        at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator.initiateService(JdbcEnvironmentInitiator.java:94)
                        at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator.initiateService(JdbcEnvironmentInitiator.java:35)
                        at org.hibernate.boot.registry.internal.StandardServiceRegistryImpl.initiateService(StandardServiceRegistryImpl.java:88)
                        at org.hibernate.service.internal.AbstractServiceRegistryImpl.createService(AbstractServiceRegistryImpl.java:254)
                        at org.hibernate.service.internal.AbstractServiceRegistryImpl.initializeService(AbstractServiceRegistryImpl.java:228)
                        at org.hibernate.service.internal.AbstractServiceRegistryImpl.getService(AbstractServiceRegistryImpl.java:207)
                        at org.hibernate.engine.jdbc.internal.JdbcServicesImpl.configure(JdbcServicesImpl.java:51)
                        at org.hibernate.boot.registry.internal.StandardServiceRegistryImpl.configureService(StandardServiceRegistryImpl.java:94)
                        at org.hibernate.service.internal.AbstractServiceRegistryImpl.initializeService(AbstractServiceRegistryImpl.java:237)
                        at org.hibernate.service.internal.AbstractServiceRegistryImpl.getService(AbstractServiceRegistryImpl.java:207)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.handleTypes(MetadataBuildingProcess.java:352)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.complete(MetadataBuildingProcess.java:111)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.build(MetadataBuildingProcess.java:83)
                        at org.hibernate.boot.internal.MetadataBuilderImpl.build(MetadataBuilderImpl.java:418)
                        at org.hibernate.boot.internal.MetadataBuilderImpl.build(MetadataBuilderImpl.java:87)
                        at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:692)
                        at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:727)
                        at HibernateSessionFactory.rebuildSessionFactory(HibernateSessionFactory.java:46)
```

**解決方法:**  
將 resin/webapp-jars/jboss-logging-3.1.0.CR2.jar 升至 jboss-logging-3.3.2.Final.jar



## NoSuchMethodError : javax.persistence.Table.indexes() ##
```sh
2018-11-05 11:54:08 [PropertyBinder.java:169] MetadataSourceProcessor property status with lazy=false
2018-11-05 11:54:08 [AbstractPropertyHolder.java:90] Attempting to locate auto-apply AttributeConverter for property [tw.com.pchomeim.leopard.db.bean.PersonalBean:status]
2018-11-05 11:54:08 [SimpleValueBinder.java:395] building SimpleValue for status
2018-11-05 11:54:08 [PropertyBinder.java:260] Building property status
[18-11-05 11:54:08.477] {resin-port-8080-29} java.lang.NoSuchMethodError: javax.persistence.Table.indexes()[Ljavax/persistence/Index;
                        at org.hibernate.cfg.annotations.EntityBinder.processComplementaryTableDefinitions(EntityBinder.java:1100)
                        at org.hibernate.cfg.AnnotationBinder.bindClass(AnnotationBinder.java:772)
                        at org.hibernate.boot.model.source.internal.annotations.AnnotationMetadataSourceProcessorImpl.processEntityHierarchies(AnnotationMetadataSourceProcessorImpl.java:245)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess$1.processEntityHierarchies(MetadataBuildingProcess.java:222)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.complete(MetadataBuildingProcess.java:265)
                        at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.build(MetadataBuildingProcess.java:83)
                        at org.hibernate.boot.internal.MetadataBuilderImpl.build(MetadataBuilderImpl.java:418)
                        at org.hibernate.boot.internal.MetadataBuilderImpl.build(MetadataBuilderImpl.java:87)
                        at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:692)
                        at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:727)
                        at HibernateSessionFactory.rebuildSessionFactory(HibernateSessionFactory.java:46)
```
**解決方法:**  
(法一)  
https://www.itread01.com/content/1533039708.html  
https://www.cnblogs.com/leinuo2016/p/9396962.html  
https://stackoverflow.com/questions/29382856/how-to-classload-java-ee-7-in-resin-4-0-42-and-not-use-the-default-javaee-16-jar  
(法二)  
將Hibernate降至4.3.11(亦能解決上一個)，並在resin.xml內加
```xml
<class-loader>
    <servlet-hack/>
</class-loader>
```
