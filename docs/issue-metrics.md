![Octocats building electronics in space.](https://github.com/github/github-ospo/assets/6935431/f57b1341-4d5f-4f32-96be-0c1049f4a5ec)

# Introducing Issue Metrics GitHub Action: metrics for issues, pull requests, and discussions

##  Data-driven insights

At GitHub, we believe that data-driven insights are the keys to success for any software development project. Understanding the health and progress of your issues, pull requests, and discussions is crucial for effective collaboration, maintainership, and project management. 

That is why we’re excited to announce the release of the [Issue Metrics GitHub Action](https://github.com/github/issue-metrics), a powerful tool that empowers developers and teams to measure key metrics and gain valuable insights into their projects.

With the new Issue Metrics GitHub Action, you can now easily track and monitor important metrics related to issues, pull requests, and discussions, such as **time to first response**, **time to close**, and more for any given time period.

### Whether you're an individual developer, a small team, or a large organization, these metrics will help you gauge the overall health, progress, and engagement of your projects.

## Sample report

![A sample report showing 2 tables. The first table contains overall metrics like average time to first response, anda corresponding value of 50 minutes and 44 seconds. The second table contains a list of the issues measured, with links to the issue and the metrics as measured on the individual issue.](https://github.com/github/github-ospo/assets/6935431/2f369265-baa8-4067-8660-24b2ba75f4cf)


## Common use cases

**Maintainers: ensuring proper attention**
As a maintainer, it is essential to give reasonable attention to the issues and pull requests in the repositories you maintain. With the Issue Metrics GitHub Action, you can track metrics, such as the number of open issues, closed issues, open pull requests, and merged pull requests. 

These metrics can provide you with a clear overview of the workload for a project over a given week, month, or even year. The action can also allow you to consider how you or your team prioritize time and attention effectively while also highlighting potentially overlooked requests in need of attention.

**First responders: timely user contact**
As a first responder in a repository, it's part of the job description to ensure that users receive contact in a reasonable amount of time. By utilizing the Issue Metrics GitHub Action, you can keep track of metrics like the number of discussions awaiting replies, unresolved issues, or pull requests waiting for reviews. These metrics enable you to maintain a high level of responsiveness, fostering a positive user experience and timely problem resolution. These can be used to build a to-do list or retrospectively to reflect on how long users had to wait for a response during a given time period.

**Open Source Program Office (OSPO): streamlining open source requests**
An important part of what OSPOs do is making the open source release process easy and efficient while adhering to company policy. This process usually involves employees opening an issue, pull request, or discussion. With the Issue Metrics GitHub Action, OSPOs can gain valuable insights into the number of requests, the ratio of open to closed requests, and metrics related to the time it takes to navigate the open-source process to completion. 

These metrics empower you to streamline your workflows, optimize response times, and ensure a smooth open-source collaboration experience. Optimizing the open source release process encourages employees to continue to produce open source projects on the organization's behalf.

**Product development teams: optimizing pull request reviews**
Product development teams rely heavily on the code review process to collaborate and build high-quality software. By leveraging the Issue Metrics GitHub Action, teams can measure metrics such as the time it takes to get pull request reviews. These insights allow you to reflect on the data during retrospectives, identify areas for improvement, and optimize the review process to enhance team collaboration and accelerate development cycles. 

### "Certain aspects of efficiency and flow may be hard to measure but often it is possible to spot and remove inefficiencies in the value stream. - Forsgren et al. 2021"

## Setup and workflow integration

Setting up the Issue Metrics GitHub Action takes a few minutes, compared to the few hours it takes to calculate these metrics manually. You also only need to set up the action once, and it will run on a regular basis of your own choosing. It integrates into your existing GitHub Actions workflow or you can create a new workflow specifically for metrics tracking. 

The action provides a wide range of customizable options, allowing you to tailor the issues, pull requests, and discussions measured by utilizing [GitHub’s powerful search filtering](https://docs.github.com/en/issues/tracking-your-work-with-issues/filtering-and-searching-issues-and-pull-requests). [Ready to use configurations](https://github.com/github/issue-metrics#example-workflows) have been tested and used internally at GitHub and are now available for you to try out as well. 

Here is one such example that runs monthly to report on metrics for issues created last month:

```yaml
name: Monthly issue metrics
on:
  workflow_dispatch:
  schedule:
    - cron: '3 2 1 * *'

jobs:
  build:
    name: issue metrics
    runs-on: ubuntu-latest
    
    steps:

    - name: Get dates for last month
      shell: bash
      run: |
        # Get the current date
        current_date=$(date +'%Y-%m-%d')

        # Calculate the previous month
        previous_date=$(date -d "$current_date -1 month" +'%Y-%m-%d')

        # Extract the year and month from the previous date
        previous_year=$(date -d "$previous_date" +'%Y')
        previous_month=$(date -d "$previous_date" +'%m')

        # Calculate the first day of the previous month
        first_day=$(date -d "$previous_year-$previous_month-01" +'%Y-%m-%d')

        # Calculate the last day of the previous month
        last_day=$(date -d "$first_day +1 month -1 day" +'%Y-%m-%d')

        echo "$first_day..$last_day"
        echo "last_month=$first_day..$last_day" >> "$GITHUB_ENV"

    - name: Run issue-metrics tool
      uses: github/issue-metrics@v2
      env:
        GH_TOKEN: ${{ secrets.GH_TOKEN }}
        SEARCH_QUERY: 'repo:owner/repo is:issue created:${{ env.last_month }} -reason:"not planned"'

    - name: Create issue
      uses: peter-evans/create-issue-from-file@v4
      with:
        title: Monthly issue metrics report
        content-filepath: ./issue_metrics.md
        assignees: <YOUR_GITHUB_HANDLE_HERE>
```

## Ready to start leveling up your GitHub project management?

Head over to the [Issue Metrics GitHub Action repository](https://github.com/github/issue-metrics) to explore the documentation, installation instructions, and examples. The repository provides a comprehensive README file that guides you through the setup process and showcases the wide range of metrics you can measure. If you need additional help, feel free to open an issue in the repository.

GitHub is committed to providing developers with the best tools to enhance collaboration and productivity. The Issue Metrics GitHub Action is a significant step towards empowering teams to measure key metrics related to issues, pull requests, and discussions. By gaining valuable insights into the pulse of your projects, you can drive continuous improvement and deliver exceptional software. We are using this in several places internally across GitHub to help us continually improve and hope this action can help you as well. Happy coding!
