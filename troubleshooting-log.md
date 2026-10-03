# Troubleshooting Log — Docker + CI/CD Project

## Incident 1: Container stopped

**What I did:** Ran `docker stop my-nginx-container` to simulate the
container going down.

**How I detected it:** Refreshed the website in the browser — it failed to
load. Running `docker ps` confirmed the container was no longer in the
running list.

**How I fixed it:** Ran `docker start my-nginx-container`.

**How I confirmed the fix:** Refreshed the browser — the site loaded again.
`docker ps` showed the container back in "Up" status.

---

## Incident 2: First GitHub Actions run failed

**What happened:** The first workflow run (triggered when the .yml file was
first created) failed immediately.

**How I diagnosed it:** Checked the Actions tab, opened the failed run, and
viewed the workflow file for that specific commit — it was empty (the file
was created before the YAML content was pasted and committed).

**Root cause:** GitHub Actions triggers on every push to main, including the
initial empty-file commit, before the real workflow content existed.

**Resolution:** The very next commit added the actual YAML content, and the
following run succeeded automatically — confirmed via the green checkmark
and a verified push to Docker Hub.

**Lesson:** Workflow files should ideally be fully written locally and
committed in one complete commit, rather than created empty and edited
afterward, to avoid triggering a run against an incomplete file.
