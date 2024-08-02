# About this
Implementation of MapReduce using both Java and Python in Hadoop to identify and calculate the word count for boardgame review data.
The link to our dataset which uploaded using the Amazon S3: https://groupassignment24.s3.amazonaws.com/bgg-15m-reviews.csv 

# Background
This project will runs on a cluster of AWS EC2 instances with the SunU-Hadoop-Image v1.3 AMI, where it is configured with 1 master node and 3 slave nodes.

## How to run?


### _Environment Setup_

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

### _Dataset Preparation_

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

### _Project Setup_
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
### _MapReduce Apporach Using *JAVA* in Hadoop_

- #### MapReduce Execution

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

- #### Output Verification
1. List the output file
```
# List the files in the output directory to verify the MapReduce job completion.
hadoop fs -ls /user/hadoop/wordcounts2
```

2. View the output content
```
# Display the content of the output file to validate the results of the word count operation.
hadoop fs -cat /user/hadoop/wordcounts2/part-r-00000
```

### _MapReduce Apporach Using *PYTHON* in Hadoop_

- #### Create Workspace
```
# Create a 'python' directory in the workspace to store the MapReduce project files.
mkdir ~/workspace/python

# Navigate to the newly created 'python' directory.
cd ~/workspace/python
```

- #### Mapper and Reducer Script Creation
1. Create mapper and reducer scripts
```
# Open nano text editor to create the 'mapper.py' script.
nano mapper.py

# Open nano text editor to create the 'reducer.py' script.
nano reducer.py
```
2. Mapper Scripts (_mapper.py_)
```
#!/usr/bin/env python3
import sys

for line in sys.stdin:
    line = line.strip()
    words = line.split()
    for word in words:
        print('%s\t%s' % (word, 1))
```
   
3. Reducer Scripts (_reducer.py_)
```
#!/usr/bin/env python3
import sys

current_word = None
current_count = 0
word = None

for line in sys.stdin:
 line = line.strip()
 word, count = line.split('\t', 1)

 try:
     count = int(count)
  except ValueError:
     continue

  if current_word == word:
      current_count += count
  else:
      if current_word:
          print('%s\t%s' % (current_word, current_count))
       current_count = count
       current_word = word

if current_word == word:
 print('%s\t%s' % (current_word, current_count))
```

- #### Mapreduce Execution
```
# Run the MapReduce job, specifying the input file, output directory, mapper, and reducer scripts.
hadoop jar /home/hadoop/hadoop-3.2.2/share/hadoop/tools/lib/hadoop-streaming-3.2.2.jar -input boardgamerev.csv -output pc4 -file mapper.py -file reducer.py -mapper mapper.py -reducer reducer.py
```

- #### Output Verification
1. List the output file
```
# List the files in the output directory to verify the MapReduce job completion.
hadoop fs -ls /user/hadoop/pc4
```

2. View the output content
```
# Display the content of the output file to validate the results of the word count operation.
hadoop fs -cat /user/hadoop/pc4/part-00000| sort -k2,2nr | more
```

