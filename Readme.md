@'

\# DevOps Internship - Task 2



\## Objective

Create a simple Jenkins CI/CD pipeline to build and deploy a Node.js app using Docker.



\## Tools Used

\- Jenkins

\- Docker

\- Node.js



\## Steps

1\. Wrote a simple Node.js web app (`app.js`).

2\. Added `package.json` for app configuration.

3\. Created a `Dockerfile` to containerize the app.

4\. Wrote a `Jenkinsfile` defining pipeline stages:

&nbsp;  - \*\*Checkout\*\*: Fetch source code.

&nbsp;  - \*\*Build\*\*: Build Docker image.

&nbsp;  - \*\*Test\*\*: Run basic npm test.

&nbsp;  - \*\*Deploy\*\*: Run the app on port 8081.

5\. Configured Jenkins to trigger the pipeline on code commits.



\## Output

Once deployed, open \[http://localhost:8081](http://localhost:8081) to see:





