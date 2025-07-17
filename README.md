# 30279

Reproduction for [Renovate issue 30279](https://github.com/renovatebot/renovate/issues/30279).

## Current behavior

1. Renovate creates new branch with submodule updated
2. github-actions bot updates some files and pushes a new commit to the same branch
3. Renovate re-runs and sees that the branch is not modified (expected behavior, since email of github-actions bot is specified in `gitIgnoredAuthors`)
4. But due to [this](https://github.com/renovatebot/renovate/blob/f3cebb9b894ffa691a948fe2cfbd93835a103098/lib/workers/repository/update/branch/get-updated.ts#L303-L308), already updated submodule treated as outdated
5. Renovate thinks that this is a new change and force pushes the same commit to the branch
6. Infinite loop


<details>

<summary>Full Logs</summary>

```plaintext
DEBUG: Dependency https://github.com/prettier/prettier has unsupported/unversioned value main (versioning=git)
DEBUG: PackageFiles.add() - Package file saved for base branch
{
  "baseBranch": "main"
}

DEBUG: Package releases lookups complete
{
  "baseBranch": "main"
}

DEBUG: No currentVersionTimestamp for prettier
DEBUG: Repository libYears
{
  "libYears": {
    "managers": {
      "git-submodules": 0,
      "github-actions": 0
    },
    "total": 0
  }
  "dependencyStatus": {
    "outdated": 1,
    "total": 3
  }
}

DEBUG: branchifyUpgrades
DEBUG: detectSemanticCommits()
DEBUG: semanticCommits: returning "disabled" from cache
DEBUG: 1 flattened updates found: prettier
DEBUG: Returning 1 branch(es)
DEBUG: config.repoIsOnboarded=true
DEBUG: packageFiles with updates
{
  "baseBranch": "main"
  "config": {
    "git-submodules": [
      {
        "datasource": "git-refs",
        "deps": [
          {
            "currentDigest": "7584432401a47a26943dd7a9ca9a8e032ead7285",
            "currentValue": "main",
            "depName": "prettier",
            "packageName": "https://github.com/prettier/prettier",
            "versioning": "git",
            "warnings": [],
            "updates": [
              {
                "updateType": "digest",
                "newValue": "main",
                "newDigest": "3ae092d6dff471ec39c9a0a7cde48b856fc5fb0c",
                "branchName": "renovate/prettier-digest"
              }
            ]
          }
        ],
        "packageFile": ".gitmodules"
      }
    ],
    "github-actions": [
      {
        "deps": [
          {
            "autoReplaceStringTemplate": "{{depName}}@{{#if newDigest}}{{newDigest}}{{#if newValue}} # {{newValue}}{{/if}}{{/if}}{{#unless newDigest}}{{newValue}}{{/unless}}",
            "commitMessageTopic": "{{{depName}}} action",
            "currentDigest": "11bd71901bbe5b1630ceea73d27597364c9af683",
            "currentValue": "v4.2.2",
            "currentVersion": "v4.2.2",
            "currentVersionAgeInDays": 266,
            "currentVersionTimestamp": "2024-10-23T14:24:28.000Z",
            "datasource": "github-tags",
            "depName": "actions/checkout",
            "depType": "action",
            "fixedVersion": "v4.2.2",
            "mostRecentTimestamp": "2024-10-23T14:24:28.000Z",
            "packageName": "actions/checkout",
            "registryUrl": "https://github.com",
            "replaceString": "actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2",
            "sourceUrl": "https://github.com/actions/checkout",
            "versioning": "docker",
            "warnings": [],
            "updates": []
          },
          {
            "autoReplaceStringTemplate": "{{depName}}-{{newValue}}",
            "currentValue": "latest",
            "datasource": "github-runners",
            "depName": "ubuntu",
            "depType": "github-runner",
            "packageName": "ubuntu",
            "replaceString": "ubuntu-latest",
            "skipReason": "invalid-version",
            "updates": []
          }
        ],
        "packageFile": ".github/workflows/update-prettier.yml"
      }
    ]
  }
}

DEBUG: detectSemanticCommits()
DEBUG: semanticCommits: returning "disabled" from cache
DEBUG: processRepo()
DEBUG: Processing 1 branch: renovate/prettier-digest
DEBUG: getBranchPr(renovate/prettier-digest)
DEBUG: findPr(renovate/prettier-digest, undefined, open)
DEBUG: Found PR #2
DEBUG: 1 PRs are currently open
DEBUG: ConcurrentPRs count = 1
DEBUG: 1 already existing branches found.
DEBUG: Branches count = 1
DEBUG: Calculating PRs created so far in this hour currentHourStart=2025-07-17T12:00:00.000Z
DEBUG: 1 PRs have been created so far in this hour.
DEBUG: HourlyPRs count = 1
DEBUG: syncBranchState() (branch="renovate/prettier-digest")
DEBUG: syncBranchState(): update branchSha (branch="renovate/prettier-digest")
DEBUG: getBranchPr(renovate/prettier-digest) (branch="renovate/prettier-digest")
DEBUG: findPr(renovate/prettier-digest, undefined, open) (branch="renovate/prettier-digest")
DEBUG: Found PR #2 (branch="renovate/prettier-digest")
DEBUG: branchExists=true (branch="renovate/prettier-digest")
DEBUG: dependencyDashboardCheck=undefined (branch="renovate/prettier-digest")
DEBUG: PR rebase requested=false (branch="renovate/prettier-digest")
DEBUG: Check for closed PR because recreating closed PRs is disabled. (branch="renovate/prettier-digest")
DEBUG: findPr(renovate/prettier-digest, Update prettier digest to 3ae092d, !open) (branch="renovate/prettier-digest")
DEBUG: prAlreadyExisted=false (branch="renovate/prettier-digest")
DEBUG: Open PR Count: 1, Existing Branch Count: 1, Hourly PR Count: 1 (branch="renovate/prettier-digest")
DEBUG: Checking if PR has been edited (branch="renovate/prettier-digest")
DEBUG: branch.isModified(): using git to calculate (branch="renovate/prettier-digest")
DEBUG: syncGit(): Initializing git repository into /tmp/renovate/repos/github/auvred/renovate-repro (branch="renovate/prettier-digest")
DEBUG: Performing blobless clone (branch="renovate/prettier-digest")
DEBUG: git clone completed (branch="renovate/prettier-digest")
{
  "durationMs": 279
}

DEBUG: latest repository commit (branch="renovate/prettier-digest")
{
  "latestCommit": {
    "hash": "a23e0d636a2b0b7aa5da2c460caa54fd23a6adab",
    "date": "2025-07-17T15:23:21+03:00",
    "message": "submodule",
    "refs": "HEAD -> main, origin/main, origin/HEAD",
    "body": "",
    "author_name": "auvred",
    "author_email": "aauvred@gmail.com"
  }
}

DEBUG: Current branch SHA: a23e0d636a2b0b7aa5da2c460caa54fd23a6adab (branch="renovate/prettier-digest")
DEBUG: branch.isModified() = false (branch="renovate/prettier-digest")
DEBUG: Found existing branch PR (branch="renovate/prettier-digest")
DEBUG: Checking schedule(schedule=at any time, tz=null, now=2025-07-17T12:26:14.156Z) (branch="renovate/prettier-digest")
DEBUG: No schedule defined (branch="renovate/prettier-digest")
DEBUG: Converting rebaseWhen=auto to rebaseWhen=behind-base-branch because automerge=true (branch="renovate/prettier-digest")
DEBUG: Branch already exists (branch="renovate/prettier-digest")
DEBUG: branch.isBehindBase(): using git to calculate (branch="renovate/prettier-digest")
DEBUG: branch.isBehindBase(): false (branch="renovate/prettier-digest")
{
  "baseBranch": "main"
  "branchName": "renovate/prettier-digest"
}

DEBUG: Branch is up-to-date (branch="renovate/prettier-digest")
DEBUG: isBranchConflicted(main, renovate/prettier-digest) (branch="renovate/prettier-digest")
DEBUG: branch.isConflicted(): using git to calculate (branch="renovate/prettier-digest")
DEBUG: Setting git author name: renovate[bot] (branch="renovate/prettier-digest")
DEBUG: Setting git author email: 29139614+renovate[bot]@users.noreply.github.com (branch="renovate/prettier-digest")
DEBUG: branch.isConflicted(): false (branch="renovate/prettier-digest")
DEBUG: Branch does not need rebasing (branch="renovate/prettier-digest")
DEBUG: Using reuseExistingBranch: true (branch="renovate/prettier-digest")
DEBUG: Setting current branch to main (branch="renovate/prettier-digest")
DEBUG: latest commit (branch="renovate/prettier-digest")
{
  "branchName": "main"
  "latestCommitDate": "2025-07-17T15:23:21+03:00"
  "sha": "a23e0d636a2b0b7aa5da2c460caa54fd23a6adab"
}

DEBUG: manager.getUpdatedPackageFiles() reuseExistingBranch=true (branch="renovate/prettier-digest")
DEBUG: updateArtifacts for updatedPackageFiles (branch="renovate/prettier-digest")
DEBUG: Updating submodule prettier (branch="renovate/prettier-digest")
DEBUG: Updated 1 package files (branch="renovate/prettier-digest")
DEBUG: Updated 1 lock files (branch="renovate/prettier-digest")
{
  "updatedArtifacts": [
    "prettier"
  ]
}

DEBUG: Getting comments for #2 (branch="renovate/prettier-digest")
DEBUG: http cache: saving https://api.github.com/repos/auvred/renovate-repro/issues/2/comments?per_page=100 (etag="2195a679a42598c72608e42373c9edcad7510ddcf496f0b99162a00130bb47f7", lastModified=undefined) (branch="renovate/prettier-digest")
DEBUG: Found 0 comments (branch="renovate/prettier-digest")
DEBUG: Getting comments for #2 (branch="renovate/prettier-digest")
DEBUG: http cache: Using cached response: https://api.github.com/repos/auvred/renovate-repro/issues/2/comments?per_page=100 from 2025-07-17T12:26:33.740Z (branch="renovate/prettier-digest")
DEBUG: Found 0 comments (branch="renovate/prettier-digest")
DEBUG: 2 file(s) to commit (branch="renovate/prettier-digest")
DEBUG: Preparing files for committing to branch renovate/prettier-digest (branch="renovate/prettier-digest")
DEBUG: git commit (branch="renovate/prettier-digest")
{
  "deletedFiles": []
  "ignoredFiles": []
  "result": {
    "author": null,
    "branch": "renovate/prettier-digest",
    "commit": "61e8b05cf8dfdce8ae5ce0ca04db3f2a954fc6a0",
    "root": false,
    "summary": {
      "changes": 1,
      "insertions": 1,
      "deletions": 1
    }
  }
}

DEBUG: resetToCommit(a23e0d636a2b0b7aa5da2c460caa54fd23a6adab) (branch="renovate/prettier-digest")
DEBUG: Fetching branch renovate/prettier-digest (branch="renovate/prettier-digest")
DEBUG: Setting current branch to main (branch="renovate/prettier-digest")
DEBUG: latest commit (branch="renovate/prettier-digest")
{
  "branchName": "main"
  "latestCommitDate": "2025-07-17T15:23:21+03:00"
  "sha": "a23e0d636a2b0b7aa5da2c460caa54fd23a6adab"
}

DEBUG: Returning PR from cache (branch="renovate/prettier-digest")
DEBUG: GitHub-native automerge: not enabled in repo settings (branch="renovate/prettier-digest")
{
  "prNumber": 2
}

DEBUG: PR platform automerge re-attempted...prNo: 2 (branch="renovate/prettier-digest")
INFO: Branch updated (branch="renovate/prettier-digest")
{
  "commitSha": "a89ac0345daf1eed9cbb55c234aab1984f6d2595"
}
```

</details>

Below is the most important part of the logs above, showing that the `prettier` submodule is added to the update list even though it's already up to date on the branch:

```plaintext
DEBUG: manager.getUpdatedPackageFiles() reuseExistingBranch=true (branch="renovate/prettier-digest")
DEBUG: updateArtifacts for updatedPackageFiles (branch="renovate/prettier-digest")
DEBUG: Updating submodule prettier (branch="renovate/prettier-digest")
DEBUG: Updated 1 package files (branch="renovate/prettier-digest")
DEBUG: Updated 1 lock files (branch="renovate/prettier-digest")
{
  "updatedArtifacts": [
    "prettier"
  ]
}

DEBUG: Getting comments for #2 (branch="renovate/prettier-digest")
DEBUG: http cache: saving https://api.github.com/repos/auvred/renovate-repro/issues/2/comments?per_page=100 (etag="2195a679a42598c72608e42373c9edcad7510ddcf496f0b99162a00130bb47f7", lastModified=undefined) (branch="renovate/prettier-digest")
DEBUG: Found 0 comments (branch="renovate/prettier-digest")
DEBUG: Getting comments for #2 (branch="renovate/prettier-digest")
DEBUG: http cache: Using cached response: https://api.github.com/repos/auvred/renovate-repro/issues/2/comments?per_page=100 from 2025-07-17T12:26:33.740Z (branch="renovate/prettier-digest")
DEBUG: Found 0 comments (branch="renovate/prettier-digest")
DEBUG: 2 file(s) to commit (branch="renovate/prettier-digest")
DEBUG: Preparing files for committing to branch renovate/prettier-digest (branch="renovate/prettier-digest")
DEBUG: git commit (branch="renovate/prettier-digest")
{
  "deletedFiles": []
  "ignoredFiles": []
  "result": {
    "author": null,
    "branch": "renovate/prettier-digest",
    "commit": "61e8b05cf8dfdce8ae5ce0ca04db3f2a954fc6a0",
    "root": false,
    "summary": {
      "changes": 1,
      "insertions": 1,
      "deletions": 1
    }
  }
}
```

## Expected behavior

Other managers (like npm) works as expected.
They detect that the branch already contains the new version of the dependency, so there is no need to push another commit.

Below are the logs for npm manager in the same situation (Renovate push, github-actions push, Renovate scan):

```plaintext
DEBUG: manager.getUpdatedPackageFiles() reuseExistingBranch=true (branch="renovate/prettier-3.x")
DEBUG: npm.updateDependency(): dependencies.prettier = ~3.6.0 (branch="renovate/prettier-3.x")
DEBUG: No package files need updating (branch="renovate/prettier-3.x")
DEBUG: Getting updated lock files (branch="renovate/prettier-3.x")
DEBUG: Writing package.json files (branch="renovate/prettier-3.x")
{
  "packageFiles": [
    "package.json"
  ]
}

DEBUG: Writing package-lock.json (branch="renovate/prettier-3.x")
DEBUG: Massaging npm lock file before writing to disk (branch="renovate/prettier-3.x")
DEBUG: Writing any updated package files (branch="renovate/prettier-3.x")
DEBUG: Found 0 npm host rule(s) (branch="renovate/prettier-3.x")
DEBUG: Found 1 host rule(s) without host type (branch="renovate/prettier-3.x")
DEBUG: Found 1 host rule(s) without host type after dropping duplicates (branch="renovate/prettier-3.x")
DEBUG: No updated lock files in branch (branch="renovate/prettier-3.x")
DEBUG: No changes to package files, skipping post-upgrade tasks (branch="renovate/prettier-3.x")
DEBUG: No files to commit (branch="renovate/prettier-3.x")
```
