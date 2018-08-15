# logstash-demo

## Oracle to Oracle

### From Oracle to STDOUT

```text
input {
  jdbc {
    jdbc_driver_library => "/root/logstash-6.2.3/lib/ojdbc6.jar"
    jdbc_driver_class => "Java::oracle.jdbc.driver.OracleDriver"
    jdbc_connection_string => "jdbc:oracle:thin:system/oracle@//10.10.11.111:49161/xe"
    jdbc_user => "system"
    jdbc_password => "oracle"

    record_last_run => "true"
    use_column_value => "false"
    tracking_column => "num"
    last_run_metadata_path => "my_info"
    clean_run => "false"

    jdbc_paging_enabled => "true"
    jdbc_page_size => "50000"
    #statement_filepath => "/usr/local/logstash/bin/logstash_jdbc_test/jdbc_oracle.sql"
    statement => "select num, rec from lgt01"
    schedule => "* * * * *"
    type => "tstype"
  }
}

filter {
  json {
    source => "message"
    remove_field => ["message"]
  }
  #grok {
    #match => { "message" => "%{COMBINEDAPACHELOG}" }
    #match => { "message" => "test" }
  #}
  date {
    match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

output {
  stdout {
    codec => json_lines
  }
}
```
