---
aliases:
  - important
  - dont-touch-it
  - frontend
---
### 1. Firstly, copy the env variables given below :
---
**PS: Make sure this document is being shared only to the trusted sources. If any mishap happens, you'll be consider responsible :)**

```ts
VITE_AUTH_URL=https://68065209.propelauthtest.com

NODE_ENV="development"

VITE_BACKEND_URL_DEVELOPMENT=

VITE_BACKEND_URL_PRODUCTION=

VITE_AZURE_CLIENT_ID=

VITE_AZURE_CLIENT_API_Key=

VITE_MAPTILER_API_KEY=nDr20tn6Gf1fy8sQc8Mp

VITE_WINDY_API_KEY=zNYzKklqIpOnE5ZLaNzHEA1r4yVF9cNp
```



### 2. Run the following commands :
---
- First, make sure you are in **Git Bash**.
- Secondly, check your directory.
- Now here are the commands : 
	- If you have node_modules : 
		`npm run reinstall`
	- If not :
		`npm install`
> It takes a lot of time. Like till then you will complete RDR2 but that thing will still take time :(  Node_modules cooked fr.
- Then, run it :
	`npm run dev`

> If there is an issue, remove the `package-lock.json` file & reinstall the packages.



### 3. Start with a new branch :

If you're gonna do any changes (_Documentation, Code changes, anything....._), Go for creating a new branch everytime.
Make sure you mention that branch to your mate beforehand. Do follow naming conventions : 
`feature/navbar` , `fix/sidebar_ui`, 
