FROM jupyter/all-spark-notebook

RUN mkdir /home/$NB_USER/spark-packages 

ADD http://dl.bintray.com/spark-packages/maven/datastax/spark-cassandra-connector/2.0.1-s_2.11/spark-cassandra-connector-2.0.1-s_2.11.jar /home/$NB_USER/spark-packages/
ADD http://central.maven.org/maven2/com/typesafe/scala-logging/scala-logging-slf4j_2.11/2.1.2/scala-logging-slf4j_2.11-2.1.2.jar /home/$NB_USER/spark-packages/
ADD http://central.maven.org/maven2/com/typesafe/scala-logging/scala-logging-api_2.11/2.1.2/scala-logging-api_2.11-2.1.2.jar /home/$NB_USER/spark-packages/
ADD http://dl.bintray.com/spark-packages/maven/graphframes/graphframes/0.5.0-spark2.1-s_2.11/graphframes-0.5.0-spark2.1-s_2.11.jar /home/$NB_USER/spark-packages/
ADD https://github.com/natalinobusa/nak/raw/master/nak_2.11-1.3.jar /home/$NB_USER/spark-packages/

USER root
RUN chown $NB_USER:users /home/$NB_USER/spark-packages/*
USER $NB_USER

# update SPARK_OPTS and reinstall toree
ENV SPARK_JARS local:///home/$NB_USER/spark-packages/spark-cassandra-connector-2.0.1-s_2.11.jar
ENV SPARK_JARS ${SPARK_JARS},local:///home/$NB_USER/spark-packages/scala-logging-slf4j_2.11-2.1.2.jar
ENV SPARK_JARS ${SPARK_JARS},local:///home/$NB_USER/spark-packages/scala-logging-api_2.11-2.1.2.jar
ENV SPARK_JARS ${SPARK_JARS},local:///home/$NB_USER/spark-packages/graphframes-0.5.0-spark2.1-s_2.11.jar
ENV SPARK_JARS ${SPARK_JARS},local:///home/$NB_USER/spark-packages/nak_2.11-1.3.jar

ENV SPARK_OPTS ${SPARK_OPTS} --conf spark.cassandra.connection.host=cassandra
ENV SPARK_OPTS ${SPARK_OPTS} --jars ${SPARK_JARS}

# Apache Toree install the kernel 
# (must be done again if you add packages or change SPARK_OPTS)
RUN echo jupyter toree install --user --spark_home=${SPARK_HOME} --spark_opts="'${SPARK_OPTS}'"
RUN jupyter toree install --user --spark_home=${SPARK_HOME} --spark_opts="'${SPARK_OPTS}'"

WORKDIR /home/$NB_USER/work

# add extra packages
ADD requirements.txt ./
RUN pip install -r ./requirements.txt

# set workdir as home
WORKDIR /home/$NB_USER

