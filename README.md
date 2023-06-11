# spark-docker
Dockerfiles for Apache Spark with various JVMs.

# Overview
This repository contains three template Dockerfiles with various JVM implementations:

* Eclipse Temurin on Ubuntu
* GraalVM CE on Ubuntu
* OpenJDK on Clear Linux

Also included:
* AWS SDK (for `s3a` support), along with Hadoop cloud connectors.
* Fully-configured OpenBLAS installation.

These Dockerfiles are fully compatible with `spark-submit` when running on Kubernetes.

# Supported version combinations
The Hadoop version is used for the Hadoop Cloud connector.

* `SPARK_VERSION=3.3.0` + `HADOOP_VERSION=3.3.2`
* `SPARK_VERSION=3.3.1` + `HADOOP_VERSION=3.3.2`
* `SPARK_VERSION=3.3.2` + `HADOOP_VERSION=3.3.2`
* `SPARK_VERSION=3.4.0` + `HADOOP_VERSION=3.3.4`

# Building
Run:

    docker build --build-arg SPARK_VERSION=SEE_ABOVE --build-arg HADOOP_VERSION=SEE_ABOVE -f Dockerfile-graalvm-ce-11-jdk-jammy -t IMAGE:TAG .

For example:

    docker build --build-arg SPARK_VERSION=3.4.0 --build-arg HADOOP_VERSION=3.3.4 -f Dockerfile-eclipse-temurin-11-jre-jammy -t spark:3.4.0-eclipse-temurin-11-jre-jammy .

# Testing Native BLAS on Spark

Run one of these commands in a terminal:

    spark-shell

    // spark 3.3.1:
    import dev.ludovic.netlib.NativeBLAS; NativeBLAS.getInstance() // should print dev.ludovic.netlib.blas.JNIBLAS@... with no warnings

    // spark 3.4.0:
    import dev.ludovic.netlib.blas.NativeBLAS; NativeBLAS.getInstance() // should print dev.ludovic.netlib.blas.JNIBLAS@... with no warnings
