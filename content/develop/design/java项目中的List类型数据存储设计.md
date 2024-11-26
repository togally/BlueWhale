+++
title = 'java项目中的List类型数据存储设计'
date = 2024-10-16T16:30:25+08:00
tags = ['mybatis']
series = ["设计方案"]
draft = false
+++
## 问题背景
 在项目开发中有时候我们会遇到一条记录中可能某条记录的某个属性对应多个值的问题，
 - 一般情况下我们是采用 “,”  拼接然后以字符串的形式存储的方案。这种方案有一个不好的地方就是需要在代码中硬编码。
 - 因此为了解决这个问题我基于<strong>数据库varchar类型 + mybatisPlus的filedHandler + TableFiled注解 </strong> 做了个小方案来解决它,
使用的话只需要在List属性上使用注解即可。

## 整体设计
![img.png](design/ListHandler.png)
- 首先我们要保证在整体java项目中除了实体类要添加注解之外，在业务代码中无感，所以要考虑在接收请求，和数据入库两个点有没有有问题。
1. 接收请求可以由web框架自动序列化，我们只需要定义List类型的属性即刻
```java
public class SysDept  {
    private List<Integer> deptType;
}
```
2. 其次在入库时数据库要实现List存储，现有的数据结构已经无法满足，所以肯定是varchar类型，因为我们只需要使用自定义Handler并配合注解来处理序列化和反序列化时的数据解析即可。
```java
@TableName(value = "sys_dept",autoResultMap = true)
public class SysDept  {
    @TableField(typeHandler = MpListHandler.class)
    private List<Integer> deptType;
}
```

## 源码
```java
// 数据处理器
@MappedTypes({List.class})
@MappedJdbcTypes(JdbcType.VARCHAR)
public class MpListHandler extends JacksonTypeHandler {

    public static final String QUERY_SPLIT = ",";


    @Override
    protected Object parse(String json) {
        json = RegExUtils.replaceAll(json, Pattern.quote("[,"), "[");
        json = RegExUtils.replaceAll(json, Pattern.quote(",]"), "]");
        return super.parse(json);
    }

    @Override
    protected String toJson(Object obj) {
        String json = super.toJson(obj);
        json = RegExUtils.replaceAll(json, Pattern.quote("["), "[,");
        json = RegExUtils.replaceAll(json, Pattern.quote("]"), ",]");
        return json;
    }

    public MpListHandler() {
        super(List.class);
    }

    public static String queryValue(Object param) {
        return QUERY_SPLIT + param + QUERY_SPLIT;
    }

    /**
     * list in 条件查询
     *
     * @param lqw
     * @param deptType
     */
    public static void listInQuery(LambdaQueryWrapper<SysDept> lqw, List<?> deptType) {
        if (CollUtil.isNotEmpty(deptType)) {
            lqw.and(t->{
                for (Object value : deptType) {
                    t.or().like(SysDept::getDeptType, MpListHandler.queryValue(value));
                }
            });
        }
    }
}
```

## 坑点
1. 实体类上注解 <strong>@TableName(value = "sys_dept",autoResultMap = true) 的autoResultMap一定要开启</strong>
2. 针对于该属性的in匹配需要考虑下，源码中给出了一个方法是拆除每个item，用like和or来匹配