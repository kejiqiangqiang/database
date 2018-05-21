--CTE�ݹ�
with recursion as(
SELECT Convert(varchar(200),'') as ParentName,ParentCode,FunctionCode,FunctionName,FunctionType,IsUse,Sort as PSort,Sort,Cast(PNodes.FunctionCode as varchar(4000)) AS PATH
  FROM #Function PNodes where PNodes.ParentCode is null--����Դ���нڵ���ɸѡ�����ڵ�
--���ڵ�  
union all
--�ڵ�
SELECT CUR.FunctionName as ParentName,Nodes.ParentCode,Nodes.FunctionCode,Nodes.FunctionName,Nodes.FunctionType,Nodes.IsUse,Cur.Sort as PSort,Nodes.Sort,Cast(CUR.PATH+'->'+Nodes.FunctionCode as varchar(4000)) AS PATH
  FROM #Function Nodes --����Դ���нڵ�
  inner join--������
  recursion CUR--�ݹ�
  on Nodes.ParentCode=CUR.FunctionCode
)

select * from recursion
order by PSort,Sort

--���Ƶݹ����
--OPTION(MAXRECURSION 100)