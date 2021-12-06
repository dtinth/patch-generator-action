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

```sh
echo '
H4sIAAAAAAACA32QT0/DMAzF7/kUj3Nx1pauYxNCRWJHOCCOHJY1ThtpJFOa8UeI705LgW0c5oPl
J9tPP1tbY0DU2Ag1kdvAMVoOocb6UAnrNL9hznmpDUuZzssZmwJZmpZFIYjoeFskSfLPoapAGZIM
VSXo41M84d7D8evGOoaKYKfhDYzdsEj6vj4Ee1je3N4t5bPuTf/qH6jp2swyzqW8TKflhcqOofbT
I9JeD0B5eV4g+c69fGzZoYuqYbz7XUDdKtdwB9Wj7TpGbG0v6mi9WwiMQUOnW0BH62I72apYt9Sw
46CiDzROVy+ZwGq1EhB0ddZz/f6FbON8YBqOJ7o+/ZSTm188f2DQyAEAAA==
' | base64 --decode | gunzip | git apply
```

If the diff is too large, the action will instead suggest you to download the patch file from the artifacts and apply it using the [`git apply`](https://git-scm.com/docs/git-apply) command.
