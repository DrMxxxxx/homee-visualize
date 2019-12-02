# homee-visualize
## Get your historical data and make it nice looking at it!

This project is itended to give the user the possibility to visualize the historical data of the famous "homee" smart home.

For easy compatibility and flexibility this system leverages the docker containerization. That means all services are running in a dedicated docker container.

### Components used are:

1. Logstash:
  For easy parsing the csv files with the historical data.

2. InfluxDB:
  This TSDB is used to store the gathered data from logstash.

3. Grafana:
  The Website is used for building graphs and gauges with the data from the InfluxDB.
  
This is "work in project", there can be changes in the future. :)

Feel free to use it!
