# HBase-SparkStreaming
Simple Spark Streaming project which reads from HBase Table and writes to HBase Table

#Prereqs to run

1) Create an hbase table to write to:
  a)Launch the hbase shell
      $hbase shell
  b) create table
      create '/user/chanumolu/sensor', {NAME=>'data'}, {NAME=>'alert'}, {NAME=>'stats'}

#Execution:

Step 1: mvn clean install

Step 2: start the streaming app

${SPARK_HOME}/bin/spark-submit --driver-class-path ${HBASE_CLASSPATH} --class examples.HBaseSensorStream hbasesparkstreamingapp-1.0.jar

Step 3: copy the streaming data file to the stream directory
cp sensordata.csv  /user/chanumolu/stream/.

Step 4: you can scan the data written to the table, however the values in binary double are not readable from the shell
launch the hbase shell,  scan the data column family and the alert column family
$hbase shell
scan '/user/chanumolu/sensor',  {COLUMNS=>['data']}
scan '/user/chanumolu/sensor',  {COLUMNS=>['alert']}

Step 5: launch one of the programs below to read data and calculatecalculate stats for one column
${SPARK_HOME}/bin/spark-submit --driver-class-path ${HBASE_CLASSPATH} --class examples.HBaseReadWrite hbasesparkstreamingapp-1.0.jar
calculate stats for whole row
${SPARK_HOME}/bin/spark-submit --driver-class-path ${HBASE_CLASSPATH} --class examples.HBaseReadRowWriteStats hbasesparkstreamingapp-1.0.jar

launch the shell and scan for statistics
hbase shell
scan '/user/chanumolu/sensor',  {COLUMNS=>['stats']}
