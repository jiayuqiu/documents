# 将多张DataFrame表写入到同一个Excel的不同sheet中

```python
writer = pd.ExcelWriter('out.xlsx')



data = pd.read_table('table_All_pivot.csv',sep=',')
data.to_excel(writer,'table_All_pivot',index=False)


sf_All = pd.read_table('sf_All.csv', sep=',' )
sf_All.to_excel(writer,'sf_All',columns=sf_All.columns[2:],index=False)


table_All_f = pd.read_csv('table_All.csv',sep=',')
table_All_f.to_excel(writer,'table_All_f',columns=table_All_f.columns[1:],index=False)


sfweibo_All_f = pd.read_csv('sfweibo_All_f.csv')
sfweibo_All_f.to_excel(writer,'sfweibo_All_f',columns=sfweibo_All_f.columns[1:],index=False)


base = pd.read_excel('test1.xlsx','Sheet2')
base.to_excel(writer,'base',index=False)

writer.save()
```

![img](https://img-blog.csdn.net/20170629112127764?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjA2Mzc3Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
