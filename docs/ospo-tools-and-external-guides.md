## Tools

These tools, whether developed by GitHub or the broader community, are useful for OSPOs to manage and scale open source initiatives effectively. While we are highlighting some of the tools and guides we encounter that solve common use cases for OSPOs, there are even more out there. To help curate a more robust toolkit, consider adding the `OSPO` [topic](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics#adding-topics-to-your-repository) to relevant repositories. See the full list of [OSPO topic repositories](https://github.com/topics/ospo). If you're looking for more tools you can also check out [`awesome-ospo`](https://github.com/todogroup/awesome-ospo) that provides a list of packages and projects OSPOs in the TODO Group have found useful.

### Compliance

Compliance tools focus on ensuring that projects adhere to licensing requirements, security standards, and policies. They are important for OSPOs to mitigate legal risks and maintain the integrity of their codebase.

- [GitHub's Dependency Graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph#supported-package-ecosystems)
- [Licensee: A Ruby Gem to detect under what license a project is distributed](https://github.com/licensee/licensee)
- [Licensed: A Ruby gem to cache and verify the licenses of dependencies](https://github.com/github/licensed)
- [GitHub API for licenses (uses `licensee` from above)](https://docs.github.com/en/rest/reference/licenses)
- [Policy as Code GitHub Action: Allow users to configure their risk threshold for security issues reported by GitHub Code Scanning, Secret Scanning and Dependabot Security.](https://github.com/marketplace/actions/ghas-policy-as-code)
- [Safe Settings: an app to manage policy-as-code and apply repository settings to repositories across an organization.](https://github.com/github/safe-settings)

### Contributions

Simplifying the process of contributing to projects by automating tasks like fork creation and reviews makes it easier for OSPOs to encourage and manage community contributions.

- [Forker: GitHub Action to automate fork creation. This action uses octokit.js and the GitHub API to automatically create a repository fork, either in your personal GitHub account or a GitHub organization that you administer](https://github.com/wayfair-incubator/forker)

### Project administration

Project administration tools are designed to streamline the setup and ongoing management of repositories. For OSPOs, these tools are essential for maintaining the health of multiple projects. They help in tasks like renaming branches, ensuring the presence of CONTRIBUTING.md files, and detecting codes of conduct, thereby standardizing project setups.

- [Changing the default branch name for GitHub repositories](https://github.com/github/renaming#renaming-existing-branches)
- [GitHub Action: Automatically open a pull request for repositories that have no CONTRIBUTING.md file](https://github.com/github/automatic-contrib-prs)
- [Code of conduct detector based off Licensee](https://github.com/benbalter/coconductor)

### Project and Organization Metrics

Metrics tools provide data-driven insights into project health and community engagement. For OSPOs, understanding these metrics is key to making informed decisions. These tools offer analytics on various aspects like code contributions, dependency graphs, and overall activity within the organization.

- [Cauldron - Software development analytics](https://cauldron.io/)
- [GitHub Organization Insights](https://docs.github.com/en/enterprise-cloud@latest/organizations/collaborating-with-groups-in-organizations/viewing-insights-for-your-organization)

### User administration

User administration tools help OSPOs manage their community and internal team members more efficiently. These tools automate the process of adding users to an organization, thereby reducing manual errors and saving time.

- [A simple Oauth app to automatically add users to an organization](https://github.com/benbalter/add-to-org)

## Community open source and OSPO guides

These guides serve as comprehensive resources for best practices, strategies, and frameworks that OSPOs can adopt. They are curated by reputable organizations and experts in the field, providing a wealth of knowledge for setting up and scaling open source programs effectively.

- TODO Group: [https://todogroup.org/guides/](https://todogroup.org/guides/)
- GitHub: [opensource.guide](opensource.guide)
- Google: [https://opensource.google/documentation/reference](https://opensource.google/documentation/reference)
- Linux Foundation: [https://www.linuxfoundation.org/resources/open-source-guides/creating-an-open-source-program](https://www.linuxfoundation.org/resources/open-source-guides/creating-an-open-source-program)
- opensource.com: [https://opensource.com/article/20/5/open-source-program-office](https://opensource.com/article/20/5/open-source-program-office)
- ben.balter.com: [https://ben.balter.com/2021/06/15/managing-open-source-communities-at-scale/](https://ben.balter.com/2021/06/15/managing-open-source-communities-at-scale/)
