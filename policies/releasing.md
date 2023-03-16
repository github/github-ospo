# Releasing an Open Source Project 🛳️

<COMPANY_NAME> encourages you to release code as an open source project. The [Release Policy](release-policy.md) details the requirements.  
The following steps should be taken to release and maintain your open source project using the MIT license:

1. **Register your release**. Create an issue in the XXX repo to register your intended release.
2. **Name your project**. Check that it does [not conflict with an existing project](http://ivantomic.com/projects/ospnc/) or infringe on any [trademarks](https://www.uspto.gov).
3. **Include the following in the root directory**. The folder with the templates is available [here](../release%20template).
   - **README.md file** describing the purpose and state of the repository. A good README file is often
     critical to project success. This is also a good place to put any top-level and important information for newcomers to your project.
   - **LICENSE.txt file** with the MIT license text. Include <COMPANY_NAME> above the copyright statement.
   - **CONTRIBUTING.md file** with instructions on contributing to your project.
     - Additionally, consider providing technical guidance like build instructions, coding conventions, or a project roadmap in the `CONTRIBUTING.md` file.
   - **CODE_OF_CONDUCT.md file**
   - Consider a **SUPPORT.md file**
   - **SECURITY.md file** including instructions that enable users to privately report security vulnerabilities
     found in your project.
4. **Prepare the code for release**.

   - **Code license**. MIT is the preferred license - other licenses must be cleared with legal.
   - **Remove sensitive assets**.
     - Remove any reference in the code to internal or confidential information, including internal paths, tools, codenames, proprietary fonts, internal telemetry and email aliases.
     - Remove any trademarks or product icons.
     - If any sensitive content or commit messages are found in commits, consider squashing the revision history.
     - To preserve sensitive history or if you wish the public repo to have zero non-commit history, rename the private repo and create a new blank slate repo with the intended public name and any commit history intended to be public.
   - **Add copyright and license headers (optional)**
     - It is best practice in many ecosystems to include a copyright statement at the top of each file. Here is one you can use at <COMPANY_NAME>:

   ```javascript
   // SPDX-FileCopyrightText: <COMPANY_NAME> and others
   // SPDX-License-Identifier: MIT
   ```

5. **Third-party Open Source**. If your repository vendors third-party OSS which is not managed/vendored by a dependency manager (e.g. `RubyGems`), describe its use and its license in a `NOTICE` file. When in doubt, consult with legal.
   - **Best Practices**.
     - Use consistent code conventions, clear function/method/variable names, and a sensible public API.
     - Keep clear comments, document intentions and edge cases.
     - Ensure the distribution mechanism is as convenient, standard, and low-overhead as possible (RubyGems, Homebrew, Bower, Maven, NuGet, etc.)
     - GitHub Actions based continuous integration that integrates with the status or checks API is ready to be enabled when the repo is made public.
     - Use [inclusive language](XXX) in your project.
6. **Publish the code**. Once your registration is complete, create an issue in `XXX` repo and request that your GitHub repository is public.
7. **Going forward**. Ensure:
   - **Staffing**. Ensure at least one team member is committed to managing community interactions merging pull requests, giving feedback, releasing new versions.
   - **Maintaining**. Make your life easier as an open source maintainer, [from documenting processes to leveraging your community](https://opensource.guide/best-practices/).
   - **Build welcoming communities**. [Build a community that encourages people](https://opensource.guide/building-community/) to use, contribute to, and share your project.
