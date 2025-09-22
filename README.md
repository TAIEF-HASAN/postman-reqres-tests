HOW TO RUN LOCALLY

1.	Clone the repo
	git clone <your-repo-url>
	cd reqres-postman
	
2.	Install dependencies (installs Newman locally)
	npm install
	
3.	Run the collection (CLI only)
	npx newman run collections/ReqRes.postman_collection.json 
	-e environments/ReqRes.postman_environment.json 
	-r cli
	
4.	Run and produce an HTML report
	npx newman run collections/ReqRes.postman_collection.json 
	-e environments/ReqRes.postman_environment.json 
	-r cli,htmlextra 
	--reporter-htmlextra-export reports/reqres-report.html
	
5.	Shortcut (npm script)
	After adding this to package.json:
	"scripts": {
	"test": "newman run collections/ReqRes.postman_collection.json -e environments/ReqRes.postman_environment.json -r cli",
	"report": "newman run collections/ReqRes.postman_collection.json -e environments/ReqRes.postman_environment.json -r cli,htmlextra --reporter-htmlextra-export reports/reqres-report.html"
	}
	Use:
	npm test # quick CLI run
	npm run report # run + generate HTML report (reports/reqres-report.html)
________________________________________

HOW CI WORKS (GitHub Actions)

•	File: .github/workflows/run-postman.yml
•	Trigger: push and pull_request on branch main
•	What the workflow does:
	1.	checks out the repo
	2.	sets up Node
	3.	installs newman + reporter
	4.	runs the Postman collection with the environment file
	5.	uploads the generated HTML report as an artifact (downloadable from the Actions run)
•	Manual re-run: Go to your repo → Actions → select the run → click “Re-run jobs” (Re-run all jobs)
________________________________________

DOWNLOAD CI REPORT (artifact)

	1.	Open the failing/passing workflow run in GitHub Actions
	2.	Scroll to the bottom → Artifacts → click newman-report → download ZIP
	3.	Unzip and open the HTML file in a browser
________________________________________

ENVIRONMENT & SECURITY

•	environments/ReqRes.postman_environment.json contains variables used by requests.
•	Do NOT commit real secrets. If you must keep a template, add: environments/ReqRes.postman_environment.example.json with placeholder values 	  and add the real one to .gitignore.
•	If the API requires an API key, keep it in environment variables (or in GitHub Actions secrets if you must run private keys in CI).
________________________________________

COMMON TROUBLESHOOTING (fast checklist)

If requests fail with getaddrinfo ENOTFOUND {{baseurl}} or unresolved variables:
• Ensure variable name case matches exactly (Postman is case-sensitive):
- Use {{baseUrl}} in requests if your env key is "baseUrl"
• Confirm the environment file path and filename are correct in the workflow:
- -e environments/ReqRes.postman_environment.json
• In your workflow: add a debug step before running newman:
- run: ls -R
This shows repository layout and proves the env file exists.
• You can bypass env-file issues by passing inline env-vars:
newman run collections/ReqRes.postman_collection.json 
--env-var "baseUrl=https://reqres.in/api" 
--env-var "userEmail=eve.holt@reqres.in" 
-r cli,htmlextra 
--reporter-htmlextra-export reports/fallback-report.html
• If newman is "command not found" locally: use local install + npx:
npm install --save-dev newman newman-reporter-htmlextra
npx newman run ...
• .github/workflows must be committed to the main branch to be picked up by Actions
• On Windows, filenames are case-insensitive locally but case-sensitive on GitHub runners — always match case exactly.
________________________________________

SUGGESTED REPO POLISH

•	Add a README badge (Actions) like:

[# ReqRes API Testing with Postman + Newman

[![CI](https://github.com/TAIEF-HASAN/postman-reqres-tests/actions/workflows/run-postman.yml/badge.svg)](https://github.com/TAIEF-HASAN/postman-reqres-tests/actions/workflows/run-postman.yml)

This project demonstrates API testing using Postman and Newman with CI/CD via GitHub Actions.


•	Add README instructions for contributors:
	o	how to run locally
	o	how to add/update collection & environment exports
	o	where to find CI artifacts
•	Keep a sample environment file: environments/ReqRes.postman_environment.example.json (no real keys)
________________________________________

QUICK NOTES / Best Practices

•	Prefer local devDependency for newman (npm install --save-dev newman) — reproducible and CI-friendly.
•	Use collection-level pre-request script to inject headers (x-api-key) so you don’t add header to every request.
•	Save dynamic IDs into environment vars via test scripts:
	pm.environment.set("createdUserId", json.id)
•	Keep reports out of repo (add reports/*.html to .gitignore) if you prefer clean commits; rely on Actions artifacts.

