# Dune Analytics入门

[Dune Analytics](https://dune.com/browse/dashboards)是进行区块链研究的强大工具。它可用于**查询，提取和可视化以太坊区块链上的大量数据**。在以太坊等公共区块链中，所有信息本来就是公共的。你所需要的只是寻找它。简单来说，**DuneAnalytics = PostgreSQL + Ethereum**，SQL查询从预先填充的数据库中查询以太坊数据，无需编写专门的脚本，只需查询数据库即可提取几乎所有驻留在区块链上的信息。

例子

```
select * from ethereum.blocks order by time limit 1;
```

![image-20231120185645464](assets\image-20231120185645464.png)

## SQL常用

```postgresql
SELECT 查询需要的字段 结果
FROM 被查询的表
WHERE 按照...条件过来
GROUP BY 按照...条件分组
HAVING 分组后过滤的条件
ORDER BY 按照...条件排序 DESC降序/ASC升序
LIMIT 返回的查询的数量
```







## References

https://qiita.com/shooter/items/3b66fc6400bc49854ffe

https://www.bilibili.com/video/BV1B24y1i7kP
