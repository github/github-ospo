# Understanding open source health metrics

![Octocat taking parts out of a box](/images/octocat-opening-box.jpeg)

Large organizations are increasingly “working in the open”: releasing internal tools and projects as open source. But once they’re out in the world, maintainers often find it difficult to track the health of these projects, especially when there are dozens or hundreds of them. GitHub’s Open Source Program Office (OSPO), in our mission to help other OSPOs and maintainers of independent OSS projects, has added a number of key metrics to our public API that can power dashboards and visualizations which help make sense of the data.

This document describes how to combine the new metrics with existing information to build a complete picture of your repository health. We'll explore both setting up a popular end-to-end solution and a DIY approach for those who want to build their own.

## Introducing Cauldron.io

Cauldron.io is a SaaS offering from Bitergia, who have built a suite of tools specifically focused on community health metrics. The free tier of the service is completely functional and provides a deep look into metrics for public repositories. The paid tiers provide additional features and support for private repositories. The metrics underpinning the cauldron.io visualizations are based on the [Community Health Analytics Open Source Software (CHAOSS) project](https://chaoss.community) metrics models and are research-supported best practices for understanding open source project health.

### Getting started with cauldron.io

To begin using Cauldron, navigate to https://cauldron.io/ and click **Login**. You'll be prompted to authenticate with a number of SAML providers; choose GitHub from the list and accept the authorization dialog that follows. Choose **Create a new report** and then **GitHub** from the list of options. You'll be prompted to choose a **GitHub owner** or **owner/repo** to analyze; input your organization name if you want the whole organization's repository analyzed or pick a specific repository to see only its stats. You can combine organizations and individual repositories, or additionally pick **Repository List** as a data source, which accepts a list of arbitrary URLs to repositories. Once your list is complete, click **Create report**.

### Understanding the cauldron.io reports

Depending on the number of data sources and the activity history in the repository, the report may take a while to run. Once it's complete, you'll have a rich set of data from the activity across the repositories you've added. The amount of data provided can be overwhelming! If you've added a whole organization, you can use the **Select a repository** dropdown to narrow the data displayed to one, or a few, relevant repositories to avoid getting lost in the numbers. In order to see a complete picture of a repository's data, your filters must include both the "GitHub" data source (the URL to the repo on github.com) and the equivalent "Git" data source, identified by the `.git` extension on the repository URL.

Here are some suggestions for patterns you might look for as you examine the data.

1. **Track issue resolution time**: Another important metric for OSPOs to track is the time it takes to resolve issues in open source projects. You can see how long it takes for issues to be closed, and identify areas where improvements could be made. A persistent and growing backlog of open issues can indicate a need for more maintainers and signal that users are not having a good experience with the project.

2. **Analyze pull request flow**: Pull requests are a key part of the open source development process, and tracking the flow of PRs over time can provide valuable insights into the health of a project. Cauldron displays PR statistics under the **Reviews** heading in several tabs. The time-series charts under **Activity** show how many pull requests are being opened and closed; for healthy projects these two lines should track pretty closely. The **Community** tab shows a user-focused view on PR submitters, to show whether the community is continuing to expand by bringing on and retaining new contributors. Under **Performance**, you can see how long it takes for a PR to be merged. This information can help OSPOs identify bottlenecks in the development process, and ensure that contributions are being reviewed and merged in a timely manner. Additionally, a low ratio of PRs accepted, as indicated in the **"Reviews closed / created ratio"** chart, indicates further investigation is needed. It could be that the project’s maintainers are being overwhelmed with low quality PRs, or perhaps they are overly critical of new contributions and need guidance on helping new community members.

3. **Identify active communities**: One of the most important metrics for any OSPO to track is the overall number of contributions coming into the project. This information can help OSPOs identify project strengths, pull request and review norms, and ensure that the most important projects are getting the attention they need. By analyzing metrics such as the number of code contributions, issue resolution time, and pull request activity for a given repository, OSPOs can determine whether the repository is still being actively developed and maintained, or if it has become stagnant. If a repository is found to be inactive, the OSPO can work with the project's stakeholders to determine whether it should be [archived](https://docs.github.com/en/repositories/archiving-a-github-repository), or if resources should be allocated to help revive it. By proactively identifying repositories that may need to be archived, OSPOs can help ensure that their organization's open source portfolio remains focused and relevant.

## Working with the GitHub API

If you're interested in building your own dashboards, you can use the GitHub API to pull the data you need. The [GraphQL API](https://docs.github.com/en/graphql) provides a flexible way to query for data, and is the recommended approach for building dashboards. The [GitHub API Explorer](https://docs.github.com/en/rest/overview/explorer) is a great way to explore the data available from the API and test out queries. Specific to community health, we in the GitHub OSPO have been working on improving the GraphQL API to add metrics which were either not available at all or required some complicated work to retrieve.

### Community standards

You can retrieve information about the [community standards documents](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) via API. This can enable quickly finding, for example, repositories in an open source organization which do not have a `CONTRIBUTING.md` or `CODE_OF_CONDUCT.md`, or which have a license that is not approved by your organization's policy.

- [ContributingGuidelines](https://docs.github.com/en/graphql/reference/objects#contributingguidelines)
- [License](https://docs.github.com/en/graphql/reference/objects#license)
- [CodeOfConduct](https://docs.github.com/en/graphql/reference/objects#codeofconduct)
- [README.md](https://docs.github.com/en/graphql/reference/objects#readme)

### Repository metrics

The Repository object in the GraphQL API is the primary location for metrics which relate to project health. These metrics are available in the `Repository` object, and while there are lots of interesting fields available, we've recently coalesced the most useful ones under the `metrics` field. 

> **Note**
> As of October 2023, the following metrics are in public beta, behind a feature flag. In order to access them, you will need to add a special header to your requests: `GraphQL-Features: ospo_metrics_api` . While they are in beta, the metrics may change and the official API documentation will not have their descriptions. We will update this document as the metrics are finalized and the feature flag is removed.

- **LastContributionDate** - The most recent date there was _any of_ the following activity: a commit to a repository’s default branch, opening an issue or discussion, answering a discussion, proposing a pull request, or submitting a pull request review. This is a good single-number metric to find projects that may be unmaintained or in need of archiving.
- **CommitCount** - A monotonically increasing count of the total number of commits pushed to the default branch of the repository. Tracking the change in this over time will give a sense of the overall activity in the repository.
- **IssueCommentCount** - Similar to CommitCount, this is a monotonically increasing count of the total number of comments on issues in the repository.

### More useful metrics

In addition to the new metrics, there's a lot of useful information tucked away in the existing GraphQL API. Many tools, like the cauldron.io example above, make use of these under the hood, and you might find them helpful in your own dashboards.

- `repository(owner:"monalisa",name:"octocat") { issues { totalCount } }` - returns the number of total issues in the repository
- `repository(owner:"monalisa",name:"octocat") { forkcount }` - the total number of forks of this repository in the fork network (i.e. including forks of forks)

More complex GraphQL queries are possible as well. For example, this query:

```graphql
    repository(owner:"voxpupuli",name:"puppetboard") {
      pullRequests(states:OPEN) { totalCount }
    }
```

Returns the number of open pull requests. Other possible states are `CLOSED` and `MERGED`. Tracking these over time is a key indicator of activity in the repository.
