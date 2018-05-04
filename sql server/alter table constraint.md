# ???????

    1.?????????
    2.??????
    3.????
***
    use [EFOS.Community]

    declare @sql varchar(Max)=''
    set @sql=' alter table [Acc_VIPBrief] 
            drop constraint '+(
                                select  name from sys.objects
                                where type ='PK' and parent_object_id=
                                (
                                    select  object_id from sys.objects
                                    where type ='U' and name = 'Acc_VIPBrief'
                                )
            )
            
    --print @sql  
    exec(@sql)

    alter table [Acc_VIPBrief] add constraint [PK_Acc_VIPBrief] PRIMARY KEY CLUSTERED 
    (
        [FileKey] ASC,
        [BlocCode] ASC,
        [ParagraphType] ASC
    )WITH (PAD_INDEX  = OFF, STATISTICS_NORECOMPUTE  = OFF, SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS  = ON, ALLOW_PAGE_LOCKS  = ON) ON [PRIMARY]
    GO

***