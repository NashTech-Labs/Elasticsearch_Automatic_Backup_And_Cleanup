pipeline {
    agent any
    environment {
        // Define the Elasticsearch server address
        ES_SERVER = "localhost:9200"
    }
    stages {
        stage('Snapshot') {
            steps {
                sh '''
                    # Generate a unique snapshot name with the current date
                    SNAPSHOT_NAME="snapshot_$(date +%Y%m%d)"

                    # Create the snapshot
                    curl -X PUT "${ES_SERVER}/_snapshot/my_backup/${SNAPSHOT_NAME}?wait_for_completion=true"
                '''
            }
        }

        stage('Cleanup') {
            steps {
                sh '''
                    # Get the list of snapshots
                    SNAPSHOTS=$(curl -s "${ES_SERVER}/_snapshot/my_backup/_all" | jq -r '.snapshots[].snapshot')

                    # If there are 15 snapshots, delete the oldest one
                    if [[ $(echo "${SNAPSHOTS}" | wc -l) -ge 15 ]]; then
                        OLDEST_SNAPSHOT=$(echo "${SNAPSHOTS}" | head -n 1)
                        curl -X DELETE "${ES_SERVER}/_snapshot/my_backup/${OLDEST_SNAPSHOT}"
                    fi
                '''
            }
        }
    }

    triggers {
        cron('H 0 * * *') // Run the pipeline every day at midnight
    }
}
