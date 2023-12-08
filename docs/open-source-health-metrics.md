# Understanding open source health metrics

![Octocat taking parts out of a box](/images/octocat-opening-box.jpeg)

Large organizations are increasingly “working in the open”: releasing internal tools and projects as open source. But once they’re out in the world, maintainers often find it difficult to track the health of these projects, especially when there are dozens or hundreds of them. As part of our mission to help other Open Source Program Offices and maintainers of independent OSS projects, GitHub’s OSPO has improved and documented parts of the GitHub API that can power dashboards and visualizations which help make sense of the data.

This document describes how to combine metrics and information from the API to build a complete picture of your repository health. We'll explore both setting up a popular end-to-end solution and a more DIY approach for those who want to build their own.

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

![Cauldron.io Performance graphs for an open source project showing time to resolution for issues and PRs](https://github.com/github/github-ospo/assets/56753/5a8fa8f6-a6a5-49aa-a982-d80ee44b2e1d)

## Working with the GitHub API

If you're interested in building your own dashboards, you can use the GitHub API to pull the data you need. The [GraphQL API](https://docs.github.com/en/graphql) provides a flexible way to query for data, and is the recommended approach for building dashboards. The [GitHub API Explorer](https://docs.github.com/en/rest/overview/explorer) is a great way to explore the data available from the API and test out queries. Specific to community health, we in the GitHub OSPO have pulled together data from disparate sources to make easier dashboarding.

### Community standards

You can retrieve information about the [community standards documents](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) via API. This can enable quickly finding, for example, repositories in an open source organization which do not have a `CONTRIBUTING.md` or `CODE_OF_CONDUCT.md`, or which have a license that is not approved by your organization's policy.

- [ContributingGuidelines](https://docs.github.com/en/graphql/reference/objects#contributingguidelines)
- [License](https://docs.github.com/en/graphql/reference/objects#license)
- [CodeOfConduct](https://docs.github.com/en/graphql/reference/objects#codeofconduct)
- [README.md](https://docs.github.com/en/graphql/reference/objects#readme)

### Querying GraphQL metrics

GitHub's GraphQL API has an absolute treasure trove of information about what's going on with your repositories. In fact, the amount and variety of data available can be a bit overwhelming, so we've selected a few key metrics that can provide insights into project and community health. The following GraphQL queries will provide point-in-time data which time-series visualization software like Grafana (see below) can turn into charts that can help identify trends over time.

- Open and closed issue counts - Track these to look for a backlog in unanswered issues
- Open and closed pull requests, including which were closed without merge - Similarly, a growing backlog of open PRs can indicate maintainer overload. Additionally, a high ratio of PRS which are closed without merge could be spam or low-quality contributions which need additional guidance.
- Date of most recent activity in the repo, including discussions, PRs, issues, commits, and releases - Archiving inactive repos can reduce maintainer burden and allow you to focus on projects which need more attention.

Try these queries in the GraphQL explorer:

```graphql
# Raw numbers related to repository activity
query RepositoryMetrics {
  repository(owner: "github", name: "docs") {
    totalIssueCount: issues {
      totalCount
    }
    openIssueCount: issues(states: [OPEN]) {
      totalCount
    }
    closedIssueCount: issues(states: [CLOSED]) {
      totalCount
    }
    openPullRequestCount: pullRequests(states: [OPEN]) {
      totalCount
    }
    closedPullRequestCount: pullRequests(states: [CLOSED]) {
      totalCount
    }
    mergedPullRequestCount: pullRequests(states: [MERGED]) {
      totalCount
    }
  }
}
```

The response will look something like:

```graphql
{
  "data": {
    "repository": {
      "totalIssueCount": { "totalCount": 2991 },
      "openIssueCount": { "totalCount": 43 },
      "closedIssueCount": { "totalCount": 2948 },
      "openPullRequestCount": { "totalCount": 24 },
      "closedPullRequestCount": { "totalCount": 5448 },
      "mergedPullRequestCount": { "totalCount": 10404 }
    }
  }
}
```

The last activity query may be better suited to periodic audits looking for activity than continuous time-series graphing. It looks like this:

```graphql
query LastActivity {
  repository(owner: "github", name: "docs") {
    updatedAt
    lastestCreatedDiscussion: discussions(
      last: 1
      orderBy: { field: CREATED_AT, direction: ASC }
    ) {
      nodes {
        createdAt
      }
    }
    latestAnsweredDiscussion: discussions(
      last: 1
      orderBy: { field: UPDATED_AT, direction: ASC }
      answered: true
    ) {
      nodes {
        updatedAt
      }
    }
    lastestPullRequest: pullRequests(
      last: 1
      orderBy: { field: CREATED_AT, direction: ASC }
    ) {
      nodes {
        createdAt
      }
    }
    lastestIssue: issues(
      last: 1
      orderBy: { field: CREATED_AT, direction: ASC }
    ) {
      nodes {
        createdAt
      }
    }
    lastestCommit: pushedAt
    latestRelease {
      createdAt
    }
  }
}
```

It returns a structure like the following, with output lines joined here for brevity:

```graphql
{
  "data": {
    "repository": {
      "updatedAt": "2023-12-05T23:44:24Z",
      "lastestCreatedDiscussion": { "nodes": [ { "createdAt": "2023-11-24T12:18:22Z" } ] },
      "latestAnsweredDiscussion": { "nodes": [ { "updatedAt": "2023-11-16T09:13:07Z" } ] },
      "lastestPullRequest": { "nodes": [ { "createdAt": "2023-12-05T23:35:24Z" } ] },
      "lastestIssue": { "nodes": [ { "createdAt": "2023-12-05T04:20:22Z" } ] },
      "lastestCommit": "2023-12-05T23:35:24Z",
      "latestRelease": { "createdAt": "2023-02-14T14:35:19Z" }
    }
  }
}
```

## Other Graphing Options

With the API reference in hand, users of other time-series visualization tools should be able to incorporate these metrics alongside your existing dashboards.

### Grafana GitHub integration

There are two separate Grafana integrations with GitHub. The first (which is the one actually called an "Integration" in their nomenclature) provides some pre-built dashboards and is more directly related to the kind of repository metrics we're talking about in this doc. However, it queries the GitHub API through the Grafana Agent, a software daemon which needs to run on a host machine like an Azure Linux instance.

The second integration is a "Data Source" in their nomenclature, which queries the API directly through a server-side service. The GitHub Data Source provides a broader set of data than just repository metrics, but it has substantial overlap with the topics in this document, so we'll focus more on it.

The GitHub Integration is available here: https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-github/ . For Grafana cloud, this requires a grafana-agent running on a host which has access to query the GitHub API and publish the results to the Grafana-hosted Prometheus instance. The integration includes a dashboard with several graphs that can be used as a starting point, including open PRs, open issues, and the number of stars and watchers of a repo.

![A Grafana dashboard of GitHub repository statistics for the cli/cli repo](https://github.com/github/github-ospo/assets/56753/b8f22a07-d581-4b58-b621-0959eefa39e2)

The GitHub Data Source's page is here: https://grafana.com/grafana/plugins/grafana-github-datasource/ . You can install it directly from Grafana Cloud. Click **Connect data**, then choose **Data sources** from the sidebar. Click **Add new data source** and search for **GitHub** in the search bar. Clicking on the result will install the data source and make it available.

![Animated gif of adding the data source through Grafana cloud](/images/metrics-doc/Grafana-add-GitHub-datasource.gif)

Once it's installed, you'll need to create a Personal Access Token so it can access the GitHub API. If you're using GHES, you can set the URL endpoint to your instance's hostname by toggling the **GitHub License** selector to **Enterprise**. Click **Save and test** , and you should soon see a confirmation that the data source is working.

![Configuring the data source from the Grafana cloud UI](/images/metrics-doc/grafana-configure-datasource.png)

You can import a sample dashboard [available here](https://grafana.com/grafana/dashboards/14000-github-default/) - From the top-level page of Grafana, click **All dashboards**, then select **Import** from the **New** pulldown in the top right of the screen. Paste that url into the text field and click **Load**. You can optionally rename or organize it into a folder here, but you must configure it by selecting the datasource we configured in the previous step, by selecting it from the picker labelled GitHub. Click **Import** and after a short loading interval you should see the dashboard load.

![Animated gif showing the previous instructions for importing the dashboard](/images/metrics-doc/Grafana-import-dashboard.gif)

By default, it will fetch stats for the `grafana/grafana` repository, but you'll probably want to point it at your own repository. This is a great starting point for additional customization and querying. Check out this [blog post about how Grafana uses the data source and dashboard](https://grafana.com/blog/2020/09/21/how-we-use-the-grafana-github-plugin-to-track-outstanding-pull-requests/) for a real-world use case.
