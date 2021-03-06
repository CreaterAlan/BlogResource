---
title: Mysql全文检索（二）停用词
date: 2020-03-21 17:40:40
tags: 搜索
categories: Mysql
---

# Mysql全文检索-停用词的使用

场景：由于在使用mysql实现检索过程中，储存字段中带有不想被检索出的词。类似html标签，这时我们需使用停用词避免一些字段的搜索

*本文相关文档相关语句为Mysql的InnoDB引擎*

- 查看InnoDB禁用列表

  ```sql
  SELECT * FROM INFORMATION_SCHEMA.INNODB_FT_DEFAULT_STOPWORD;
  ```

- 定义自己的停用词列表，首先需要定义一个与INNODB_FT_DEFAULT_STOPWORD具有相同机构的表，使用停用词填充它

  ```sql
  INSERT INTO my_stopwords(value) VALUES ('Ishmael');
  ```

- 建表

  ```sql
  CREATE TABLE opening_lines (
  id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
  opening_line TEXT(500),
  author VARCHAR(200),
  title VARCHAR(200)
  ) ENGINE=InnoDB;
  ```

- 写入数据

  ```sql
  INSERT INTO opening_lines(opening_line,author,title) VALUES
  ('Call me Ishmael.','Herman Melville','Moby-Dick'),
  ('A screaming comes across the sky.','Thomas Pynchon','Gravity\'s Rainbow'),
  ('I am an invisible man.','Ralph Ellison','Invisible Man'),
  ('Where now? Who now? When now?','Samuel Beckett','The Unnamable'),
  ('It was love at first sight.','Joseph Heller','Catch-22'),
  ('All this happened, more or less.','Kurt Vonnegut','Slaughterhouse-Five'),
  ('Mrs. Dalloway said she would buy the flowers herself.','Virginia Woolf','Mrs. Dalloway'),
  ('It was a pleasure to burn.','Ray Bradbury','Fahrenheit 451');
  ```

- 替换全局停用词表

  ```sql
  SET GLOBAL innodb_ft_server_stopword_table = 'test/my_stopwords';
  ```
  在本地环境验证时发现报错，[Err] 1231 - Variable 'innodb_ft_server_stopword_table' can't be set to the value of 'test/my_stopwords'添加"@"后可用，如下

  ```sql
  SET GLOBAL innodb_ft_server_stopword_table = @'test/my_stopwords';
  ```

- 新建索引

  ```sql
  CREATE FULLTEXT INDEX idx ON opening_lines(opening_line);
  ```

- 测试是否成功

  ```sql
  SET GLOBAL innodb_ft_aux_table='test/opening_lines';
  ```

  ```sql
  SELECT word FROM INFORMATION_SCHEMA.INNODB_FT_INDEX_TABLE LIMIT 15;
  ```

  

本文引用自

[Mysql中文文档]: https://www.docs4dev.com/docs/zh/mysql/5.7/reference/fulltext-stopwords.html#full-text-%E5%81%9C%E7%94%A8%E8%AF%8D

