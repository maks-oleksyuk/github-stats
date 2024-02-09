<h1 align='center'>
   <a href='//github.com/jstrieb/github-stats'>GitHub Stats Visualization</a>
</h1>

<p align='center'>
   <a href='//github.com/jstrieb/github-stats' title='User statistics'>
      <picture>
        <source media='(prefers-color-scheme: dark)' srcset='https://github.com/jstrieb/github-stats/blob/master/generated/overview.svg#gh-dark-mode-only'>
        <img alt='User overview statistics' src='https://github.com/jstrieb/github-stats/blob/master/generated/overview.svg#gh-light-mode-only'>
      </picture>
      <picture>
        <source media='(prefers-color-scheme: dark)' srcset='https://github.com/jstrieb/github-stats/blob/master/generated/languages.svg#gh-dark-mode-only'>
        <img alt='User languages statistics' src='https://github.com/jstrieb/github-stats/blob/master/generated/languages.svg#gh-light-mode-only'>
      </picture>
   </a>
</p>

Generate visualizations of GitHub user and repository statistics with GitHub
Actions. Visualizations can include data for both private repositories.

Generated images automatically switch between the GitHub light and dark theme.

## 🌱 Background

When someone views a profile on GitHub, it is often because they are curious
about a user's open-source projects and contributions. Unfortunately, the user's
stars, forks, and pinned repositories do not necessarily reflect the
contributions they make to private repositories. The data does not present a
complete picture of the user's total contributions beyond the current year.

This project aims to collect a variety of profile and repository statistics
using the GitHub API. It then generates images that can be displayed in
repository READMEs, or a user's [Profile README].

Since the project runs on GitHub Actions, no server is required to regularly
regenerate the images with updated statistics. Likewise, since the user runs the
analysis code themselves via GitHub Actions, they can use their GitHub access
token to collect statistics on private repositories that an external service
would be unable to access.

## ⚠️ Disclaimer

If the project is used with an access token that has sufficient permissions to
read private repositories, it may leak details about those repositories in error
messages. For example, the `aiohttp` library—used for asynchronous API
requests—may include the requested URL in exceptions, which can leak the name of
private repositories. If there is an exception caused by `aiohttp`, this
exception will be viewable in the Actions tab of the repository fork, and anyone
may be able to see the name of one or more private repositories.

Due to some issues with the GitHub statistics API, there are some situations
where it returns inaccurate results. Specifically, the repository view count
statistics and total lines of code modified are probably somewhat inaccurate.
Unexpectedly, these values will become more accurate over time as GitHub caches
statistics for your repositories. Additionally, repositories that were last
contributed to more than a year ago may not be included in the statistics due to
limitations in the results returned by the API.

For more information on inaccuracies, see issues [#2], [#3], and [#13].

## 🛠️ Installation

1. Create a personal access token (not the default GitHub Actions token) using
   the instructions [here][access-token]. The Personal access token must have
   permissions: `read:user` and `repo`. Copy the access token when it is
   generated – **if you lose it, you will have to regenerate the token**.

   > Some users report that it can take a few minutes for the personal access
   > token to work. For more, see [#30].

2. Go to the **`Settings`** tab of the repository and go to the **`Secrets`**
   page (bottom left).

3. Create a new secret with the name `ACCESS_TOKEN` and paste the copied
   personal access token as the value.

4. Set up a GitHub Action for generate images:

   ```yml
   - name: 🖼 Generate images
     uses: jstrieb/github-stats@v1
     with:
       # Required for access to user statistics.
       access_token: ${{ secrets.TOKEN }}
   ```

5. Save the generated photos to the output branch. Pictures are saved to the `generated` directory

   ```yml
   - name: 📤 Push to the output branch
     uses: crazy-max/ghaction-github-pages@v4
     with:
       build_dir: generated
       target_branch: output
       commit_message: 'Update generated images'
     env:
       GITHUB_TOKEN: ${{ secrets.TOKEN }}
   ```

6. To add your statistics to your GitHub Profile README, copy and paste the
   following lines of code into your markdown content. Change the `username`, `repo` and `branch`
   value to yours.

   ```html
   <p align='center'>
     <a href='//github.com/jstrieb/github-stats' title='My statistics'>
       <picture>
         <source media='(prefers-color-scheme: dark)' srcset='https://raw.githubusercontent.com/username/repo/branch/overview.svg#gh-dark-mode-only'/>
         <img alt='My overview statistics' src='https://raw.githubusercontent.com/username/repo/branch/overview.svg#gh-light-mode-only'/>
       </picture>
       <picture>
         <source media='(prefers-color-scheme: dark)' srcset='https://raw.githubusercontent.com/username/repo/branch/languages.svg#gh-dark-mode-only'/>
         <img alt='My languages statistics' src='https://raw.githubusercontent.com/username/repo/branch/languages.svg#gh-light-mode-only'/>
       </picture>
     </a>
   </p>
   ```

## ➡️ Available options

| Options                | Description                                                                                        |
| :--------------------- | :------------------------------------------------------------------------------------------------- |
| `access_token`         | The GitHub token used to create an authenticated client. Defaults to github provided token.        |
| `excluded_repos`       | Ignore certain repositories, add them in the owner/name format (for example, jstrieb/github-stats) |
| `excluded_langs`       | Ignore certain languages (for example, html tex)                                                   |
| `exclude_forked_repos` | Exclude forks from the statistics. Default is true                                                 |

The values `excluded_repos` and `excluded_langs` must be filled in the following format:

```yml
with:
  excluded_repos: |
    jstrieb/github-stats
  excluded_langs: |
    html
    tex
```

## 🤝 Support the Project

There are a few things you can do to support the project:

- ⭐️ Star the repository (and follow me on GitHub for more)
- 📣 Share and upvote on sites like Twitter, Reddit, and Hacker News
- 🐞 Report any bugs, glitches, or errors that you find

These things motivate me to keep sharing what I build, and they provide
validation that my work is appreciated! They also help me improve the project.
Thanks in advance!

If you are insistent on spending money to show your support, I encourage you to
instead make a generous donation to one of the following organizations. By
advocating for Internet freedoms, organizations like these help me to feel
comfortable releasing work publicly on the Web.

- [Electronic Frontier Foundation](//supporters.eff.org/donate)
- [Signal Foundation](//signal.org/donate)
- [Mozilla](//donate.mozilla.org/en-US)
- [The Internet Archive](//archive.org/donate)

### 📚 Related Projects

- Inspired by a desire to improve upon
  [anuraghazra/github-readme-stats](//github.com/anuraghazra/github-readme-stats)
- Makes use of [GitHub Octicons](//primer.style/octicons) to precisely
  match the GitHub UI

[Profile README]: //docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme
[access-token]: //docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token
[#2]: //github.com/jstrieb/github-stats/issues/2
[#3]: //github.com/jstrieb/github-stats/issues/3
[#13]: //github.com/jstrieb/github-stats/issues/13
[#30]: //github.com/jstrieb/github-stats/issues/30
