# ELK
ELK is the combination of 3 open source products

1 - Elastic Search : It is used to store and process logs
2 - Logstash : It is used to collect application logs and store in Elastic Search
3 - Kibana : It will provide user interface to monitor application logs

ELK Setup
1 - Download ELK Softwares

	1 - Elastic Search : https://www.elastic.co/downloads/elasticsearch
	2 - Kibana : https://www.elastic.co/downloads/kibana
	3 - Logstash : https://www.elastic.co/downloads/logstash


2 - Extract all zip files

3 - Run elasticsearch using elasticsearch.bat file (make sure all security settings disable in elasticsearch.yml before running)

 	1 - cmd run file = elasticsearch.bat


4 - Check Elastic Search Running or not (URL : http://localhost:9200/ )

5 - Run kibana using kibana.bat file (before running kibana, enable elasticsearch url in kibana.yml file)

 	1 - cmd run file = kibana.bat
	2 - check enable or not enabled = elasticsearch.hosts: ["http://localhost:9200"]

	Kibana security configuration
	Open the Kibana configuration file (kibana.yml) located in the Kibana installation directory 	(example like usuallyin = C:\path\ELKSoftwares\kibana-8.15.2\config).
	Locate the following settings and set them to unique, random values:
	
	1 - xpack.security.encryptionKey
	2 - xpack.encryptedSavedObjects.encryptionKey
	3 - xpack.reporting.encryptionKey (if you plan on using Reporting features)

6 - Check Kibana running or not ( URL : http://localhost:5601/app/home )

7 - Run Spring Boot Application and generate log file with log messages

8 - add in logstash-sample.conf file like below

input {
  file {
    path => "C:/Users/service1/logs/service1.log"
    start_position => "beginning"
    tags => ["service1"]
  }
  
  file {
    path => "C:/Users/service2/logs/service2.log"
    start_position => "beginning"
    tags => ["service2"]
  }

  file {
    path => "C:/Users/service3/logs/service3.log"
    start_position => "beginning"
    tags => ["service3"]
  }
}

filter {
  # Additional filters can be applied here if needed
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "microservices-logs-%{+YYYY.MM.dd}"
  }
  
  stdout { codec => rubydebug }
}

9 - Run logstash server using below command
	logstash -f logstash-sample.conf
	cmd>logstash -f C:\logstash\logstash-8.15.2\config\logstash-sample.conf

10 - Check logstash server is running or not ( http://localhost:9600 )

11 - Check application logs in Kibana dashboard
