# logstash-simple-config-file
Configuring Logstash

To configure Logstash, you create a config file that specifies which plugins you want to use and settings for each plugin. You can reference event fields in a configuration and use conditionals to process events when they meet certain criteria. When you run logstash, you use the -f to specify your config file.

Let’s step through creating a simple config file and using it to run Logstash. Create a file named "logstash-simple.conf" and save it in the same directory as Logstash.

input { stdin { } }
output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
}

How to Import from CSV into Elasticsearch via Logstash and Sincedb

input {
  file {
    path => "/home/timo/bitcoin-data/*.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ","
#Date,Open,High,Low,Close,Volume (BTC),Volume (Currency),Weighted Price
     columns => ["Date","Open","High","Low","Close","Volume (BTC)", "Volume (Currency)" ,"Weighted Price"]
  }
}
output {
   elasticsearch {
     hosts => "http://localhost:9200"
     index => "bitcoin-prices"
  }
stdout {}
}

