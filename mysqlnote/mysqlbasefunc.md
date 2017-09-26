# 记录mysql的基本操作

```python
import pymysql  
  
  
##查询  
def select():  
    conn = pymysql.connect(user='root', passwd='你的密码',  
                     host='localhost', db='test',charset='utf8')  
    cur = conn.cursor()  
    cur.execute("SELECT * FROM user")  
    for r in cur:  
          print("row_number:" , (cur.rownumber) )          
          print("id:"+str(r[0])+" name:"+str(r[1])+" password:"+str(r[2]))   
    cur.close()      
    conn.close()  
  
##插入  
def insert(name,pwd):  
    conn = pymysql.connect(user='root', passwd='你的密码',  
                     host='localhost', db='test',charset='utf8')  
    cur = conn.cursor()  
    sql= "INSERT INTO user (Name,Password) VALUES ('"+name+"','"+pwd+"')"  
    print(sql)  
    sta=cur.execute(sql)  
    if sta==1:  
        print('Done')  
    else:  
        print('Failed')     
    conn.commit()  
    cur.close()      
    conn.close()  
  
##更新  
def update(name,pwd):  
    conn = pymysql.connect(user='root', passwd='你的密码',  
                     host='localhost', db='test',charset='utf8')  
    cur = conn.cursor()  
    sql= "UPDATE USER SET PASSWORD='"+pwd+"' WHERE NAME='"+name+"'""'" 
    print(sql) 
    sta=cur.execute(sql) 
    if sta==1: 
        print('Done') 
    else: 
        print('Failed')    
    conn.commit() 
    cur.close()     
    conn.close() 
 
##删除 
def delete(name): 
    conn = pymysql.connect(user='root', passwd='你的密码', 
                     host='localhost', db='test',charset='utf8') 
    cur = conn.cursor() 
    sql = "DELETE FROM USER WHERE Name='"+name+"'"  
    print(sql)  
    sta=cur.execute(sql)  
    if sta==1:  
        print('Done')  
    else:  
        print('Failed')     
    conn.commit()  
    cur.close()      
    conn.close()  
  
##调用函数进行操作即可  
```

****
