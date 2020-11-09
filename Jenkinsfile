node {

env.PATH=env.PATH+":/opt/gradle-6.7/bin:/root/.sdkman/candidates/groovy/3.0.6/bin"
    def student = "rovsyannikov"
    def child_job = "MNTLAB-${student}-child1-build-job"
    def child_artifact = "${student}_dsl_script.tar.gz"

    stage 'Preparation (Checking out)'
        git url: "https://github.com/RomanOvsyannikov/mntlab-pipeline", branch: 'rovsyannikov'
