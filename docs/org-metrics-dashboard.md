# Five ways that Organization Metrics Dashboard drives open source success

![Octocat taking parts out of a box](/images/octocat-opening-box.jpeg)

Large organizations are increasingly “working in the open”: releasing internal tools and projects as open source. But once they’re out in the world, maintainers often find it difficult to track the health of these projects, especially when there are dozens or hundreds of them. GitHub’s Open Source Program Office (OSPO), in our mission to help other OSPOs and maintainers of independent OSS projects, developed an Organization Metrics Dashboard to address this problem.

The Dashboard aims to provide a comprehensive view of an organization's open source activity. The dashboard collects and displays metrics related to code contributions, issue resolution, and pull requests. Here are a few ways OSPOs can use this tool to understand what’s happening across the open source projects in their organization.

![Organization metrics dashboard](/images/organization-metrics-dashboard.png)

1. **Identify active communities**: One of the most important metrics for any OSPO to track is the overall number of contributions coming into the project. In the Organization Metrics Dashboard, it's easy to see what types of contributions are coming in and how often. This information can help OSPOs identify project strengths, pull request and review norms, and ensure that the most important projects are getting the attention they need. The header columns of the data table will re-sort the table based on that column so you can focus on the most active – or the most dusty and abandoned – projects for deeper examination.

2. **Track issue resolution**: Another important metric for OSPOs to track is the time it takes to resolve issues in open source projects. You can see how long it takes for issues to be closed, and identify areas where improvements could be made. This information can help OSPOs allocate resources more effectively, and ensure that critical issues are being addressed in a timely manner.

3. **Analyze pull requests**: Pull requests are a key part of the open source development process, and can provide valuable insights into the health of a project. With organization metrics, OSPOs can see how many pull requests are being opened and closed, and how long it takes for them to be merged. This information can help OSPOs identify bottlenecks in the development process, and ensure that contributions are being reviewed and merged in a timely manner. Additionally, the dashboard displays a “PR merge ratio” as a percentage: how many PRs were closed by merging versus those closed without merge. A low percentage of PRs accepted indicates some investigation is needed. It could be that the project’s maintainers are being overwhelmed with low quality PRs, or perhaps they are overly critical of new contributions and need guidance on helping new community members.

4. **Ensure best practices**: The Community Standards section provides a set of best practice guidelines for open source projects, covering topics such as code of conduct, licensing, and documentation. This helps OSPOs confirm that their projects have these guidelines in place to safeguard and sustain the community. The filter input box supports the `license:` condition, which allows you to quickly find repositories using licenses that might not be approved by your organization’s legal department.

   ![License filtering results](/images/license-filtering.png)

5. **Curate open source portfolio**: By analyzing metrics such as the number of code contributions, issue resolution time, and pull request activity for a given repository, OSPOs can determine whether the repository is still being actively developed and maintained, or if it has become stagnant. If a repository is found to be inactive, the OSPO can work with the project's stakeholders to determine whether it should be [archived](https://docs.github.com/en/repositories/archiving-a-github-repository), or if resources should be allocated to help revive it. By proactively identifying repositories that may need to be archived, OSPOs can help ensure that their organization's open source portfolio remains focused and relevant.

Another important component of gaining insights and context around the metrics you are seeing is knowing how they have changed over time. To view metrics over time for a specific repository, click the graph icon next to the repository name to open the repository activity view. You can change the timescale and period to get a zoomed-out view over time (though note that the metrics only go back 12 months due to data retention).

![Contribution activity graph](/images/contribution-activity.png)

When OSPOs notice a metric that warrants further investigation, they can dive deeper and gain understanding of what may be causing the issue. This time-based context is critical for understanding how organizational priorities, staffing changes, or other time-based factors may be affecting a given open source project.

Overall, [GitHub's Organization Metrics Dashboard feature](https://docs.github.com/en/early-access/insights/organization-metrics-dashboard) provides a powerful set of tools for tracking and analyzing open source metrics. OSPOs can use these insights to identify areas for improvement and allocate resources more effectively, and help drive open source success for their organization.

The Organization Metrics Dashboard is currently in private beta and subject to change. If you are managing a large organization’s open source efforts and are interested in joining the beta to provide product feedback during early development, please email ospo@github.com to request enablement for your organization.
