# Understanding open source health metrics

![Octocat taking parts out of a box](/images/octocat-opening-box.jpeg)

Large organizations are increasingly “working in the open”: releasing internal tools and projects as open source. But once they’re out in the world, maintainers often find it difficult to track the health of these projects, especially when there are dozens or hundreds of them. As part of our mission to help other Open Source Program Offices and maintainers of independent OSS projects, GitHub’s OSPO has improved and documented parts of the GitHub API that can power dashboards and visualizations which help make sense of the data.

This document describes how to combine metrics and information from the API to build a complete picture of your repository health. We'll explore both setting up a popular end-to-end solution and a more DIY approach for those who want to build their own.

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
