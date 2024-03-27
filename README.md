Demonstrating a misleading error message caused by a missing dependency in a package manager with strict/non-hoisted installation layout.

This repro uses `yarn` v4 with `nodeLinker: pnpm`, but the same issue would happen with `pnpm` itself.

1. Clone the repo and `cd` in
2. Verify `yarn -v` shows 4.1.1
3. `yarn`
4. `yarn test` => observe that tests run but there's an "Invalid testPattern" message
5. `yarn test other` => observe that all tests run, not only the selected test

The actual issue is that `jest-util` is missing a dep on `jest-regex-util` (and due to the non-hoisted layout, it's not implicitly present), but the warning message it causes is very misleading:

```
$ yarn test
  Invalid testPattern  supplied. Running all tests instead.
  Invalid testPattern  supplied. Running all tests instead.
 PASS  ./other.test.js
 PASS  ./index.test.js

Test Suites: 2 passed, 2 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        0.346 s, estimated 1 s
Ran all test suites.

$ yarn test other
  Invalid testPattern other supplied. Running all tests instead.
  Invalid testPattern other supplied. Running all tests instead.
 PASS  ./other.test.js
 PASS  ./index.test.js

Test Suites: 2 passed, 2 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        0.27 s, estimated 1 s
Ran all test suites.
```
