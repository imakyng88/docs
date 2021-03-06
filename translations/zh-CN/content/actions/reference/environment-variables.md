---
title: 环境变量
intro: '{% data variables.product.prodname_dotcom %} 为每个 {% data variables.product.prodname_actions %} 工作流程运行设置默认环境变量。 您也可以在工作流程文件中设置自定义环境变量。'
product: '{% data reusables.gated-features.actions %}'
redirect_from:
  - /github/automating-your-workflow-with-github-actions/using-environment-variables
  - /actions/automating-your-workflow-with-github-actions/using-environment-variables
  - /actions/configuring-and-managing-workflows/using-environment-variables
versions:
  free-pro-team: '*'
  enterprise-server: '>=2.22'
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}

### 关于环境变量

{% data variables.product.prodname_dotcom %} 设置适用于工作流程运行中每个步骤的默认环境变量。 环境变量区分大小写。 在操作或步骤中运行的命令可以创建、读取和修改环境变量。

要设置自定义环境变量，您需要在工作流程文件中指定变量。 您可以使用 [`jobs.<job_id>.steps.env`](/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idstepsenv)、[`jobs.<job_id>.env`](/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idenv) 和 [`env`](/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#env) 关键字定义步骤、作业或整个工作流程的环境变量。 更多信息请参阅“[{% data variables.product.prodname_dotcom %} 的工作流程语法](/articles/workflow-syntax-for-github-actions/#jobsjob_idstepsenv)”。

```yaml
steps:
  - name: Hello world
    run: echo Hello world $FIRST_NAME $middle_name $Last_Name!
    env:
      FIRST_NAME: Mona
      middle_name: The
      Last_Name: Octocat
```

You can also use the {% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@2.22" %}`GITHUB_ENV` environment file{% else %} `set-env` workflow command{% endif %} to set an environment variable that the following steps in a workflow can use. The {% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@2.22" %}environment file{% else %} `set-env` command{% endif %} can be used directly by an action or as a shell command in a workflow file using the `run` keyword. 更多信息请参阅“[{% data variables.product.prodname_actions %} 的工作流程命令](/actions/reference/workflow-commands-for-github-actions/#setting-an-environment-variable)”。

### 默认环境变量

强烈建议操作使用环境变量访问文件系统，而非使用硬编码的文件路径。 {% data variables.product.prodname_dotcom %} 设置供操作用于所有运行器环境中的环境变量。

| 环境变量                 | 描述                                                                                                                                                                                                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CI`                 | 始终设置为 `true`。                                                                                                                                                                                                                                                                               |
| `HOME`               | 用于存储用户数据的 {% data variables.product.prodname_dotcom %} 主目录路径。 例如 `/github/home`。                                                                                                                                                                                                            |
| `GITHUB_WORKFLOW`    | 工作流程的名称。                                                                                                                                                                                                                                                                                    |
| `GITHUB_RUN_ID`      | {% data reusables.github-actions.run_id_description %}
| `GITHUB_RUN_NUMBER`  | {% data reusables.github-actions.run_number_description %}
| `GITHUB_ACTION`      | 操作唯一的标识符 (`id`)。                                                                                                                                                                                                                                                                            |
| `GITHUB_ACTIONS`     | 当 {% data variables.product.prodname_actions %} 运行工作流程时，始终设置为 `true`。 您可以使用此变量来区分测试是在本地运行还是通过 {% data variables.product.prodname_actions %} 运行。                                                                                                                                             |
| `GITHUB_ACTOR`       | 发起工作流程的个人或应用程序的名称。 例如 `octocat`。                                                                                                                                                                                                                                                            |
| `GITHUB_REPOSITORY`  | 所有者和仓库名称。 例如 `octocat/Hello-World`。                                                                                                                                                                                                                                                         |
| `GITHUB_EVENT_NAME`  | 触发工作流程的 web 挂钩事件的名称。                                                                                                                                                                                                                                                                        |
| `GITHUB_EVENT_PATH`  | 具有完整 web 挂钩事件有效负载的文件路径。 例如 `/github/workflow/event.json`。                                                                                                                                                                                                                                   |
| `GITHUB_WORKSPACE`   | {% data variables.product.prodname_dotcom %} 工作空间目录路径。 The workspace directory is a copy of your repository if your workflow uses the [actions/checkout](https://github.com/actions/checkout) action. 如果不使用 `actions/checkout` 操作，该目录将为空。 例如 `/home/runner/work/my-repo-name/my-repo-name`。 |
| `GITHUB_SHA`         | 触发工作流程的提交 SHA。 例如 `ffac537e6cbbf934b08745a378932722df287a53`。                                                                                                                                                                                                                               |
| `GITHUB_REF`         | 触发工作流程的分支或标记参考。 例如 `refs/heads/feature-branch-1`。 如果分支或标记都不适用于事件类型，则变量不会存在。                                                                                                                                                                                                                 |
| `GITHUB_HEAD_REF`    | 仅为复刻的仓库设置。 头部仓库的分支。                                                                                                                                                                                                                                                                         |
| `GITHUB_BASE_REF`    | 仅为复刻的仓库设置。 基础仓库的分支。                                                                                                                                                                                                                                                                         |
| `GITHUB_SERVER_URL`  | 返回 {% data variables.product.product_name %} 服务器的 URL。 当 {% data variables.product.prodname_actions %} 运行工作流程时，始终设置为 `true`。                                                                                                                                                              |
| `GITHUB_API_URL`     | 返回 API URL。 返回 {% data variables.product.product_name %} 服务器的 URL。 例如：`https://github.com`。                                                                                                                                                                                                 |
| `GITHUB_GRAPHQL_URL` | 返回 GraphQL API URL。 例如：`https://api.github.com/graphql`。                                                                                                                                                                                                                                    |

### 环境变量命名约定

{% note %}

**注：** {% data variables.product.prodname_dotcom %} 会保留 `GITHUB_` 环境变量前缀供 {% data variables.product.prodname_dotcom %} 内部使用。 设置有 `GITHUB_` 前缀的环境变量或密码将导致错误。

{% endnote %}

您设置的指向文件系统上某个位置的任何新环境变量都应该有 `_PATH` 后缀。 `HOME` 和 `GITHUB_WORKSPACE` 默认变量例外于此约定，因为 "home" 和 "workspace" 一词已经暗示位置。
