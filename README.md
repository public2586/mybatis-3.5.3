**MyBatis 源码解析**
=====================================


![mybatis](http://mybatis.github.io/images/mybatis-logo.png)


1、mybatis-config.xml
   
   例如：
   
    private void parseConfiguration(XNode root) {
       try {
         //解析＜properties＞ 节点
         propertiesElement(root.evalNode("properties"));
         //解析＜settings＞节点
         Properties settings = settingsAsProperties(root.evalNode("settings"));
         loadCustomVfs(settings);//设置 vfsimpl 字段
         loadCustomLogImpl(settings);
         //解析＜typeAliases＞节点
         typeAliasesElement(root.evalNode("typeAliases"));
         //解析＜plugins＞节点
         pluginElement(root.evalNode("plugins"));
         //解析＜objectFactory＞节点
         objectFactoryElement(root.evalNode("objectFactory"));
         //解析＜objectWrapperFactory＞节点
         objectWrapperFactoryElement(root.evalNode("objectWrapperFactory"));
         //解析＜reflectorFactory＞节点
         reflectorFactoryElement(root.evalNode("reflectorFactory"));
         settingsElement(settings);//将settings值设置到 Configuration中
         // read it after objectFactory and objectWrapperFactory issue #631
         //解析＜environments＞节点
         environmentsElement(root.evalNode("environments"));
         //解析＜databaseIdProvider＞节点
         databaseIdProviderElement(root.evalNode("databaseIdProvider"));
         //解析＜typeHandlers＞节点
         typeHandlerElement(root.evalNode("typeHandlers"));
         //解析＜mappers＞节点
         mapperElement(root.evalNode("mappers"));
       } catch (Exception e) {
         throw new BuilderException("Error parsing SQL Mapper Configuration. Cause: " + e, e);
       }
     }
2、**Mapper.XML
      
      例如：
      
      private void configurationElement(XNode context) {
         try {
           //获取＜mapper>节点的namespace属性
           String namespace = context.getStringAttribute("namespace");
           //如果节点的namespace属性为空，则抛出异常
           if (namespace == null || namespace.equals("")) {
             throw new BuilderException("Mapper's namespace cannot be empty");
           }
           //设置 MapperBuilderAssistant的currentNamespace 字段，记录当前命名空间
           builderAssistant.setCurrentNamespace(namespace);
           //解析<cache-ref>节点
           cacheRefElement(context.evalNode("cache-ref"));
           //解析<cache>节点
           cacheElement(context.evalNode("cache"));
           parameterMapElement(context.evalNodes("/mapper/parameterMap"));
           //解析＜resultMap＞节点
           resultMapElements(context.evalNodes("/mapper/resultMap"));
           //解析＜sql＞节点
           sqlElement(context.evalNodes("/mapper/sql"));
           //解析select|insert|update|delete等SQL节点
           buildStatementFromContext(context.evalNodes("select|insert|update|delete"));
         } catch (Exception e) {
           throw new BuilderException("Error parsing Mapper XML. The XML location is '" + resource + "'. Cause: " + e, e);
         }
       }

  
