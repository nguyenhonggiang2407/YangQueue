# Repository migration — 04/11/2026

## Source and destination

- Source repository: [nguyenhonggiang2407/DTDD](https://github.com/nguyenhonggiang2407/DTDD).
- Source branch: [`yangqueue`](https://github.com/nguyenhonggiang2407/DTDD/tree/yangqueue).
- Source commit: [`47f73b10fb9d6607ed21987bbc3980276773fa66`](https://github.com/nguyenhonggiang2407/DTDD/commit/47f73b10fb9d6607ed21987bbc3980276773fa66).
- Source tree: `9e5cf663fcde1c79cab1259ff573063e44abca29`; seven reachable commits before migration.
- Destination: [nguyenhonggiang2407/YangQueue](https://github.com/nguyenhonggiang2407/YangQueue), branch `main`.

The extraction used `git clone --single-branch --branch yangqueue`, with complete
reachable history rather than a shallow clone. The local branch was renamed to
`main`, and the source remote to `upstream`. No source branch, commit, data or
repository was deleted or rewritten. Publishing uses an ordinary push to the new
repository; the original DTDD branch remains available.

## Preservation and migration changes

The source commit remains an ancestor. Its original commit objects, authors,
dates and messages are retained. Before committing migration work, `git diff`
confirmed no change in `lib/`, `public/`, `test/`, `server.mjs`, dependency manifests,
Dockerfile, Compose, LICENSE, NOTICE or `upstream/`.

The MIT license blob remains `84ee68d33c9b68bae6c139f591996f021a02b23a` and NOTICE
remains `24993a8e8be16c52181879ce12c662cf89433923`. The original patchy631 attribution,
reference snapshot and preserved upstream source are unchanged. Read [NOTICE](../NOTICE.md)
for the distinction between reference material and YangQueue implementation.

Migration changes are limited to README clone/current-repository links, workflow
branch filters targeting `main`, this record and screenshots from the browser
verification below. The README still documents the same queue, database and limits.
The existing CI retains backend, browser, Docker health and screenshot checks.

History can be checked after publication:

```sh
git merge-base --is-ancestor 47f73b10fb9d6607ed21987bbc3980276773fa66 main
git cat-file -t 47f73b10fb9d6607ed21987bbc3980276773fa66
git diff 47f73b10fb9d6607ed21987bbc3980276773fa66 main -- lib public test server.mjs package.json package-lock.json LICENSE NOTICE.md upstream
```

The first command should return 0; the second should return `commit`; the final
command should show no business-source, dependency or attribution change from extraction.

## Validation evidence

- Baseline at the exact source SHA: 33 backend/domain/store/worker/HTTP tests passed
  on Node.js 24.19.0 during the 03/11/2026 audit. They are not rerun here merely for
  documentation changes; the new repository CI is configured to run them again.
- In the extracted repository on 04/11/2026, `npm ci` succeeded and reported zero
  dependency advisories at installation time.
- `npm run test:ui` passed all four Chromium tests against the real local server,
  in-memory SQLite and actual worker threads: queued processing and hash-checked CSV
  download; cancellation/deletion; invalid-input recovery; mobile upload, safe rendering
  and navigation at 390 px.
- The test regenerated `desktop.png`, `mobile.png` and `result.png` in `docs/screenshots/`.
  These are captures of the running app with fictional sample data; desktop and mobile
  layouts were visually inspected. No screenshot or product activity was synthesized.
- `git diff --check`, changed-file formatting and source/history preservation checks
  were performed before the migration commit.

Docker is not available through PATH or the checked Docker Desktop/Rancher Desktop
locations in this session, so no local Docker build/run is claimed. The new GitHub
Actions run and Docker smoke test must be inspected after the destination is published;
they had not run in that repository when this migration record was prepared.

There is no verified public deployment for YangQueue. The application remains a
single-node learning/personal workspace with the privacy, persistence and scaling
limits described in the README. Migration does not add billing or hosting resources.
