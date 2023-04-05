# sysbench-mongodb-lua


 <h2>How to setup enviroment</h2>

  <h3>Ubuntu Xenial(16.04)</h3>

 * install the latest sysbench
   https://github.com/akopytov/sysbench

   !!! That important that you use the latest available version of sysbench !!!

 * install prerequisites for mongorover driver
   <pre>bash# apt-get install libmongoc-dev libbson-dev luarocks</pre>

 * install mongorover driver
   <pre>bash# luarocks install mongorover --local</pre>

 <h2>How to run test</h2>

 = MongoDB specific options
 <pre>
  --mongodb_host=STRING         MongoDB: hostname [localhost]
  --mongodb_port=STRING         MongoDB: port [27017]
  --mongodb_db=STRING           MongoDB: database name [sbtest_test]
  --mongodb_password=STRING     MongoDB: password [test]
  --mongodb_db=STRING           MongoDB: database name [sbtest_test]  
</pre>
* prepare

<pre>./sysbench  oltp-mongo.lua --tables=10 --threads=10 --table-size=100 --mongodb-db=sbtest --mongodb-host=localhost --mongodb-port=27017  --rand-type=pareto prepare</pre>

* run oltp_rw
<pre>./sysbench  oltp-mongo.lua --tables=10 --threads=10 --table-size=100 --mongodb-db=sbtest --mongodb-host=localhost --mongodb-port=27017 --time=120 --report-interval=1 --rand-type=pareto run</pre>
<pre>

Running the test with following options:
Number of threads: 10
Initializing random number generator from current time


Initializing worker threads...

Threads started!


General statistics:
    total time:                          120.0106s
    total number of events:              100774

Latency (ms):
         min:                                 10.30
         avg:                                 11.91
         max:                                 25.85
         95th percentile:                     14.21
         sum:                            1199874.10

Threads fairness:
    events (avg/stddev):           10077.4000/30.69
    execution time (avg/stddev):   119.9874/0.00

 </pre>

The metric is number of events(queries/transactions) per second: 100774/120 ~ 840 events per second

* run oltp_ro test
<pre>./sysbench  oltp-mongo.lua --tables=10 --threads=10 --table-size=100 --mongodb-db=sbtest --mongodb-host=localhost --mongodb-port=27017 --time=120 --read_only=on --report-interval=1 --rand-type=pareto run</pre>

* run point_select only query
<pre>./sysbench  oltp-mongo.lua --tables=10 --threads=10 --table-size=100 --mongodb-db=sbtest --mongodb-host=localhost --mongodb-port=27017 --time=120  --point_selects=1 --simple_ranges=0 --sum_ranges=0 --order_ranges=0 --distinct_ranges=0 --index_updates=0 --non_index_updates=0  --delete_inserts=0  --report-interval=1 --rand-type=pareto run
</pre>

* cleanup
 <pre>./sysbench  oltp-mongo.lua --tables=10 --threads=10 --table-size=100 --mongodb-db=sbtest --mongodb-host=localhost --mongodb-port=27017 cleanup</pre>
