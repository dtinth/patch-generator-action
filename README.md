# patch-generator-action

Frustrated by builds failing due to trivial, auto-fixable issues? Use this action to generate a shell command to apply changes.

This action is built as part of GitHub Actions Hackathon 2021. [See the article for more details.](https://dt.in.th/go/patch-generator-action/article)

## How to use

Instead of failing the builds on issues that are auto-fixable, attempt to fix it in your workflow:

```diff
-      - run: yarn install --frozen-lockfile
-      - run: yarn lint
-      - run: yarn test
-      - run: yarn prettier --check .
+      - run: yarn install
+      - run: yarn lint --fix
+      - run: yarn test --updateSnapshots
+      - run: yarn prettier --write .
```

Then stage your changes and use this action:

<!-- prettier-ignore-start -->

```yaml
      - run: git add --update
      - uses: dtinth/patch-generator-action@v1
```

<!-- prettier-ignore-end -->

When there are auto-fixable issues, the action will generate a shell command to apply the changes, which looks like this:

```
🚨 Your branch contains outdated files.
💡 This problem can be automatically fixed.

👉 Download and extract the 'git-patch' artifact from <URL> then run: 'git apply /path/to/fix.diff --index'

👉 Alternatively, run the following command:

echo '
H4sIAAAAAAACA71XbVPbOBD+nl+xk+lNkiZWEgKBeo4OTEtL524od+U+HTeMYsuxiiP5JJmE4fjv
t5KMk4DDS689PmBb2t1H++ybEvMkgSCYcgO0T/CRFpP+XKrLJJNz3Z9RUdCMXM8ymDy63eAiZgsY
............................................................................
' | base64 --decode | gunzip | git apply --index

> See diff contents
  diff --git a/.github/workflows/manual.yml b/.github/workflows/manual.yml
  index 0274b57..6146ab2 100644
  --- a/.github/workflows/manual.yml
  +++ b/.github/workflows/manual.yml
  @@ -9,23 +9,23 @@ jobs:
     build:
       runs-on: ubuntu-latest
       steps:
  -    - uses: actions/checkout@v1
  +      - uses: actions/checkout@v1
..........................................
Error: Process completed with exit code 1.
```

If the diff is too large, the action will instead suggest you to download the patch file from the artifacts and apply it using the [`git apply`](https://git-scm.com/docs/git-apply) command.
