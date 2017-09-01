node {
  deleteDir()
  stage("Checkout Code") {
    checkout scm
  }

  def awscli = docker.image('awscli')

  awscli.inside {
    stage("Connect to AWS") {
      sh "ecs-cli configure --region us-west-2 --access-key ${AWS_ACCESS_KEY_ID} --secret-key ${AWS_SECRET_ACCESS_KEY} --cluster myblog-cluster"
    }

    stage("Create Cluster") {
      sh "ecs-cli up --keypair aws-keypair-test --capability-iam --size 2 --instance-type t2.micro --force"
    }

    stage("Push to cluster") {
      sh "ecs-cli compose up"
    }
  }

}
