# Elasticsearch Daily Snapshot and Cleanup Jenkins Pipeline

This repository contains a Jenkinsfile that automates the process of creating Elasticsearch snapshots and cleaning up old snapshots.

## Overview

The Jenkinsfile defines a pipeline with two stages:

1. **Snapshot**: This stage runs a shell script to create a snapshot of the Elasticsearch indices. The snapshot is named with the current date and is stored in a snapshot repository on the Elasticsearch server.

2. **Cleanup**: This stage runs a shell script to delete snapshots that are older than 15 days. The script retrieves the list of all snapshots from the Elasticsearch server and deletes the ones that match the criteria.

The pipeline is scheduled to run every day at midnight.

## Prerequisites

- A running Jenkins server
- A running Elasticsearch server
- Necessary permissions to create and delete snapshots in Elasticsearch and to create and configure jobs in Jenkins

## Configuration

You'll need to replace `localhost:9200` with the address of your Elasticsearch server. You'll also need to replace `my_backup` with the name of your snapshot repository.

The pipeline is configured to run every day at midnight. You can change this by modifying the cron expression in the `triggers` section of the Jenkinsfile.

## Usage

To use this Jenkinsfile, you simply need to create a new Pipeline job in Jenkins and specify this Jenkinsfile as the Pipeline script.

Once the job is set up, it will run automatically according to the schedule.
