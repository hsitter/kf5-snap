env.DIST = 'xenial'
env.TYPE = 'user'

// FIXME: should really create a cleaningNode library to do the finally shit
node('master') {
  stage 'generate'
  try {
    git 'https://github.com/apachelogger/kf5-snap'
    sh 'echo $PWD'
    sh '~/tooling/kci/contain.rb rake generate'
    sh "echo '----snapcraft----'; cat snapcraft.yaml; echo '----snapcraft----'"
    stash 'snapcraft.yaml'
  } finally {
    cleanWorkspace()
  }
}

node {
  try {
    stage ('snapcraft')
    unstash 'snapcraft.yaml'
    sh '~/tooling/kci/contain.rb rake snapcraft'
  } finally {
    cleanWorkspace()
  }
}

def cleanWorkspace() {
  step([$class: 'WsCleanup', cleanWhenFailure: true])
}
