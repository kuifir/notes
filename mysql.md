### 1.设置默认值且非空，插入时未设置字段，报错 not null

#### 问题描述

数据库设置创建时间字段，默认值为当前时间,并且非空。

```sql
 `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
```

mybatis 插入语句，插入的字段包括该字段，但是未设置插入字段

```xml

<sql id="EnclosureTable">
    office_enclosure
</sql>
<sql id="EnclosureInsertFields">
        (#{item.id},#{item.path},#{item.taskId},#{item.alias},#{item.taskDescription},#{item.size},#{item.creatorName},#{item.md5},#{item.documentType},#{item.createTime,jdbcType=TIMESTAMP},#{item.updateTime,jdbcType=TIMESTAMP})
</sql>
    <insert id="addEnclosureList">
        insert into
        <include refid="EnclosureTable"/>
        (<include refid="EnclosureFields"/>)
        values
        <foreach collection="list" item="item" index="index" separator=",">
            <include refid="EnclosureInsertFields"/>
        </foreach>
    </insert>
```

插入字段未设置时，实体类中的属性默认赋值null,插入时报错  `Column 'create_time' cannot be null`

#### 原因+解决方式

原因是 explicit*defaults*for_timestamp 参数，在mysql8里面默认变为了ON。改成off就行了。

```sql
show variables like '%explicit_defaults_for_timestamp%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| explicit_defaults_for_timestamp | ON    |
+---------------------------------+-------+
1 row in set (0.01 sec)
```

解决方式：将explicit*defaults*for_timestamp改为off，后重启mysql

```sql
 set persist explicit_defaults_for_timestamp=OFF;
  Query OK, 0 rows affected (0.00 sec)
```

