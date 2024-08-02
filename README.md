# About this
Implementation of MapReduce using both Java and Python in Hadoop to identify and calculate the word count for boardgame review data.

# Background
This project will runs on a cluster of AWS EC2 instances with the SunU-Hadoop-Image v1.3 AMI, where it is configured with 1 master node and 3 slave nodes.

## How to run?


> ### Environment Setup

 1. Switch to Hadoop user
```
# Switch to the Hadoop user to gain necessary permissions for Hadoop administration tasks.
sudo su - hadoop
```

 2. Format the Hadoop Distributed File System (HDFS) namenode
```
# Format the HDFS namenode to set up the filesystem for data storage across the cluster.
hdfs namenode -format
```

 3. Start HDFS and YARN services
```
# Start all Hadoop services, including HDFS and YARN, to ensure the system is fully operational.
start-all.sh
```

4. Navigate to course directory
```
# Change directory to the course-specific directory for subsequent operations.
cd IST3134/
```

### Dataset Preparation

1. Download the dataset (Boardgame)
```
# Download the dataset from the specified URL to the local system.
wget https://groupassignment24.s3.amazonaws.com/boardgamerev.csv
```

2. Verify the download
```
# List the details of the downloaded file to ensure it was successfully downloaded.
ls -latrh boardgamerev.csv
```

3. Transfer the dataset to HDFS
```
# Upload the dataset to HDFS for distributed storage and processing.
hadoop fs -put boardgamerev.csv /user/hadoop/

# Verify the dataset is successfully uploaded to the HDFS directory.
hadoop fs -ls /user/hadoop/
```

### Project Setup
1. Unzip the word count project file
```
# Unzip the project archive containing the word count program files.
unzip wordcount.zip
```

2. Create workspace
```
# Create a dedicated workspace directory for the MapReduce project.
mkdir ~/workspace

# Copy the word count project files to the new workspace.
cp -r wordcount/ ~/workspace/
```

3. Navigate to source directory
```
# Change to the source directory containing the Java code for the word count program.
cd ~/workspace/wordcount/src

# List the contents of the stubs directory to confirm the presence of necessary files.
ls stubs
```
### MapReduce Apporach Using *JAVA* in Hadoop

#### MapReduce Execution
1. Check the Hadoop classpath
```
# Ensure all necessary Hadoop libraries and dependencies are available.
hadoop classpath
```

2. Run the MapReduce job
```
# Execute the MapReduce job, specifying the input dataset and output directory.
hadoop jar wc.jar stubs.WordCount /user/hadoop/boardgamerev.csv wordcounts2
```

#### Output Verification
1. Check the Hadoop classpath
```
# List the files in the output directory to verify the MapReduce job completion.
hadoop fs -ls /user/hadoop/pc3
```

2. Run the MapReduce job
```
# Display the content of the output file to validate the results of the word count operation.
hadoop fs -cat /user/hadoop/pc3/part-00000
```
