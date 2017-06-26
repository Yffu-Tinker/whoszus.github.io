### 背景介绍
- 写了个springboot服务用于同步某个系统的数据，简称A系统；
- 通过一个system A 自编的唯一编号（SYSTEMID）查询数据后存到数据库；
- 数据时效性需求较高，所以需要定时任务同步数据，同步周期较短；
- 数据情况： 数据量大，通过SYSTEMID拉过来的数据出现多次重复；
- 直接调用JPA save方法持久化；
```
<S extends T> Iterable<S> save(Iterable<S> var1);
```

背景介绍完毕.. #


### JPA Save 方法解析

注：自顶向下地分析save(Iterable<S> var1),重点在于找到重复的ID判断在JPA调用的哪一个阶段；
分析方法：
```
/**
* 批量保存的JPA实现； 实际上也是的多次调用sava(S entity);
**/
 @Transactional
    public <S extends T> List<S> save(Iterable<S> entities) {
        ArrayList result = new ArrayList();
        if(entities == null) {
            return result;
        } else {
            Iterator var3 = entities.iterator();

            while(var3.hasNext()) {
                Object entity = var3.next();
                result.add(this.save(entity));
            }

            return result;
        }
    }
```


```
    /**
    * S save(S entity)实现方式
    **/
    @Transactional
    public <S extends T> S save(S entity) {
        if(this.entityInformation.isNew(entity)) {
            this.em.persist(entity);
            return entity;
        } else {
            return this.em.merge(entity);
        }
    }
    
    /**
    * 判断实体是否已经存在
    **/
    public boolean isNew(T entity) {
        if(this.versionAttribute != null && !this.versionAttribute.getJavaType().isPrimitive()) {
            DirectFieldAccessFallbackBeanWrapper wrapper = new DirectFieldAccessFallbackBeanWrapper(entity);
            Object versionValue = wrapper.getPropertyValue(this.versionAttribute.getName());
            return versionValue == null;
        } else {  
            return super.isNew(entity);
        }
    }
    
    
    
    
```


