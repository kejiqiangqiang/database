--CTE递归
with recursion as(
SELECT Convert(varchar(200),'') as ParentName,ParentCode,FunctionCode,FunctionName,FunctionType,IsUse,Sort as PSort,Sort,Cast(PNodes.FunctionCode as varchar(4000)) AS PATH
  FROM #Function PNodes where PNodes.ParentCode is null--数据源所有节点中筛选出根节点
--根节点  
union all
--节点
SELECT CUR.FunctionName as ParentName,Nodes.ParentCode,Nodes.FunctionCode,Nodes.FunctionName,Nodes.FunctionType,Nodes.IsUse,Cur.Sort as PSort,Nodes.Sort,Cast(CUR.PATH+'->'+Nodes.FunctionCode as varchar(4000)) AS PATH
  FROM #Function Nodes --数据源所有节点
  inner join--内连接
  recursion CUR--递归
  on Nodes.ParentCode=CUR.FunctionCode
)

select * from recursion
order by PSort,Sort

--限制递归次数
--OPTION(MAXRECURSION 100)