# circleci-sandbox

## タグを打った時だけビルド

`v〇〇` という名前のタグかmasterブランチをpushしたときだけデプロイまでおこない、それ以外の場合はビルドだけ。

* [branch: deploy-only-when-tagged](https://github.com/oohira/circleci-sandbox/blob/deploy-only-when-tagged/.circleci/config.yml)

```yaml
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:latest
    steps:
      - checkout
      - ...
      - deploy:
          name: deploy build artifact
          command: |
            if [ "${CIRCLE_TAG}" != "" -o "${CIRCLE_BRANCH}" == "master" ]; then
              [ "${CIRCLE_TAG}" != "" ] && TAG=${CIRCLE_TAG} || TAG=latest
              echo "Deploy! ${TAG}"
            fi
workflows:
  version: 2
  build-and-deploy:
    jobs:
      - build:
          filters:
            tags:
              only: /v.*/
```
