# mysql_tcp_monitor

该小工具通过实时获取数据包的方式，分析并解析出每次执行的sql语句、执行状态和执行时间，insert和update语句对数据做了格式化操作，update语句的where条件保留了数据，select、delete语句未做格式化，可以放于应用端、中间价层、mysql服务器上，在不影响mysql本身性能的情况下获取所有语句的基本情况，可做审计作用，需求的包：
    dpkt、psutil、pypacp

执行方式：
    python tcp_dump.py -h 可以获取参数介绍
    
比如我在中间件层对后面3306端口的数据流进行监听： python tcp_dump.py -e eth0 -p 3306 -t src
如果是对本地端口进行监听，比如我们中间件层端口为6001： python tcp_dump.py -e eth0 -p 6001 -t des

打印数据格式如下：
    source_host: 10.1.1.1 source_port: 55294 destination_host: 10.1.1.2 destination_port: 3306 sql: select * from t1 values: None execute_time:0.00028  status:#42000SELECT command denied to user 'test'@'10.1.1.1' for table 't1'

特别提示： 由于是实时获取包分析，如果数据流非常大的话，会占用不少的cpu时间
