# Python链接sql server

```python
# coding:utf-8

import pymssql
import pandas as pd

if __name__ == "__main__":
    # 不添加database参数，就会进入默认的database
    conn = pymssql.connect(host='***', user='sa', password='***')
    cur = conn.cursor()
    cur.execute("""select * from KAJH""")
    data = cur.fetchall()
    df = pd.DataFrame(list(data))
    df.to_csv("/home/qiu/Documents/ys_ais/KAJH.csv", index=None)
    if not cur:
        raise (NameError, "数据库连接失败")
    conn.close()
```
