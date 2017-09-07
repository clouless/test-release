sh 'curl -sSLko pipeline-helper.groovy ${K8S_INFRASTRUCTURE_BASE_URL}pipeline-helper/pipeline-helper.groovy?v16'
def pipelineHelper = load("./pipeline-helper.groovy")
pipelineHelper.nodejsTemplate {
  stage('prepare tools') {
    pipelineHelper.npmWriteClientConfig()
    sh 'yarn -version'
  }
  stage('git clone') {
    sh 'git clone --single-branch --branch $GWBT_BRANCH https://${GITHUB_AUTH_TOKEN}@github.com/${GWBT_REPO_FULL_NAME}.git source'
  }
  stage('release') {
    dir ('source') {
      def releaseId = pipelineHelper.githubCreateGitHubRelease("clouless", env.GWBT_REPO_NAME, "1.0", "master")
      pipelineHelper.githubUploadAssetToRelease("clouless", env.GWBT_REPO_NAME, "1.0", releaseId, "dist/", "my_asset.json", 'application/json')
    }
  }
}
