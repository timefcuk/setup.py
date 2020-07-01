# Pipeline doc

## Preparations
```
fly --target demo login --concourse-url http://127.0.0.1:8080 -u test -p test
fly --target demo sync
```

## Master pipeline
```
fly -t demo sp -c generic-pipeline.yaml -p library-master --load-vars-from master-config.yaml --load-vars-from secrets.yaml 
fly -t demo unpause-pipeline -p library-master
fly -t demo unpause-job --job library-master/build
```

## Release pipeline
```
fly -t demo sp -c generic-pipeline.yaml -p library-release --load-vars-from release-config.yaml --load-vars-from secrets.yaml
fly -t demo unpause-pipeline -p library-release
fly -t demo unpause-job --job library-release/build
```

## Feature pipeline
```
export BRANCH=fb-test
fly -t demo sp -c generic-pipeline.yaml -p library-${BRANCH} --load-vars-from feature-config.yaml --load-vars-from secrets.yaml -v ci_branch=${BRANCH}
fly -t demo unpause-pipeline -p library-${BRANCH}
fly -t demo unpause-job --job library-${BRANCH}/build
```