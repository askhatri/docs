# Appache Livy Installation

### Steps to install Apache Liy

Those steps are tested on Alpine Linux with root access. The installation directory is /root.

Step 1: Download Spark.
```shell
curl -L --retry 3 -o "spark-2.2.3-bin-hadoop2.7.tgz" "https://archive.apache.org/dist/spark/spark-2.2.3/spark-2.2.3-bin-hadoop2.7.tgz"
```
Step 2: Download Apache Livy.
```shell
curl -L --retry 3 -o "apache-livy-0.7.1-incubating-bin.zip" "https://archive.apache.org/dist/incubator/livy/0.7.1-incubating/apache-livy-0.7.1-incubating-bin.zip"
```
Step 3: Extract Spark zip file.
```shell
tar zxvf spark-2.2.3-bin-hadoop2.7.tgz
```
Step 4: Extract Livy zip file.
```shell
unzip apache-livy-0.7.1-incubating-bin.zip
```
Step 5: Configure Spark Home environment variable.
```shell
export SPARK_HOME=/root/spark-2.2.3-bin-hadoop2.7
```
Step 6: Create Livy logs folder.
```shell
mkdir /root/apache-livy-0.7.1-incubating-bin/logs
```
Step 7: Check if the Java 8 is installed not use `java -version` command. Java 8 can be installed using following command.
```shell
apk add openjdk8
```
Step 8: Change directory to Livy bin.
```shell
cd /root/apache-livy-0.7.1-incubating-bin/bin
```
Step 9: Start Livy Server in background.
```shell
./livy-server &
```
Step 10: Test the server is running file.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions| jq
```
Step 11: Stopping livy server.
```shell
ps aux | grep livy-server
kill -9 <process-id>
```
<hr>

### URLs

Livy UI: http://127.0.0.1:8998/ui

Play with Docker: https://labs.play-with-docker.com

<hr>

### Curl commands to use Livy REST APIs

Get sessions.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions| jq
```
Create session.
```shell
curl --no-progress-meter -X POST -H "Content-Type: application/json" --data '{"kind": "spark"}' http://127.0.0.1:8998/sessions| jq
```
Create statement.
```shell
curl --no-progress-meter -X POST  -d '{ "kind": "spark", "code": "sc.parallelize(1 to 10).count()" }'  -H "Content-Type: application/json" \
http://127.0.0.1:8998/sessions/0/statements | jq
```
Get statement.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions/0/statements/0 | jq
```
Create another statement.
```shell
curl --no-progress-meter -X POST  -d '{ "kind": "spark", "code": "sc.version" }'  -H "Content-Type: application/json" \
http://127.0.0.1:8998/sessions/0/statements | jq
```
Get statement.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions/0/statements/1 | jq
```
Create another session.
```shell
curl --no-progress-meter -X POST --data '{"kind": "pyspark"}' -H "Content-Type: application/json" http://127.0.0.1:8998/sessions | jq
```
Get session details.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions/1 | jq
```
Create statement in another session.
```shell
curl --no-progress-meter -X POST -H 'Content-Type: application/json' -d '{"code":"2 + 2"}' http://127.0.0.1:8998/sessions/1/statements | jq
```
Get statement details.
```shell
curl --no-progress-meter http://127.0.0.1:8998/sessions/1/statements/0 | jq
```
