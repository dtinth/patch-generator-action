# patch-generator-action

Frustrated by builds failing due to trivial, auto-fixable issues? Use this action to generate a shell command to apply changes.

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