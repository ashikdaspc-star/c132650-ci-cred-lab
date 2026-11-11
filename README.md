# C-132650 deepen lab — CI deploy job credential surface

Self-owned throwaway lab repository created for a defensive security measurement
(task `deepen#5185`, project `/evo/mssql`).

It reproduces the *documented* "deploy the PR's dacpac to a preview database" CI shape:

* `pull_request_target` — the workflow definition comes from the base branch
  (a PR author cannot change it), secrets are available;
* `actions/checkout` of `github.event.pull_request.head.repo.full_name` /
  `...head.sha` — i.e. the artefact under deploy is PR content;
* official Microsoft `sqlpackage /a:Publish` with the SQL connection string on the
  command line.

`main` carries a benign `.dacpac` (negative control). The follow-up PR from a
**second, unrelated GitHub account** (no access to this repository) carries a
`.dacpac` + same-directory .NET DLL pair, which is the delivery form described in
the report. The DLL only writes a measurement file (`/tmp/c132650-marks/*.json`)
and prints it; it does not modify this repository beyond creating a throwaway
`c132650-proof-*` branch reference (proof of the job token's write scope).
No production system, no third-party target, no user data is involved.
