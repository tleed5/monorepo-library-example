# monorepo-library-example
### Example on how to use Lerna, Github packages and NPM in a monorepo

## Authentication 
Before you can access Github packages you have to authenticate with Github using a [personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

Personally I found updating the global `.npmrc` file with the personal access token the easiest way, [see this guide](https://docs.github.com/en/packages/guides/configuring-npm-for-use-with-github-packages#authenticating-to-github-packages) on how to achieve this.

## Publishing
Ideally this part would be handled by CI/CD(Jenkins, CircleCI etc) but to do it locally you can do the following: 
1. Ensure that the main `lerna.json` has the following set: 
```
"command": {
	"publish": {
		"ignoreChanges": ["ignored-file", "*.md"],
		"registry": "https://npm.pkg.github.com"
	}
},
``` 
This makes the `lerna publish` command point towards Github packages
2.  Ensure that your personal access token has `write:packages` permission
3. Run `npm run publish` this will run `lerna publish --no-git-tag-version --no-push` which will publish each defined package to Github packages

Notes: 
This setup will always attempt to publish all packages since `--no-git-tag-version` and `--no-push` flag is used, if these weren't included Lerna's default behavior would kick in, and commit changes to the `package.json``s and set a git tag which it can then use to see if there has been any changes made to the packages

## Installing 
1. Ensure that your personal access token has `read:packages`permission
2. Run the following `npm install --registry=https://npm.pkg.github.com @tleed5/foo` this will install the latest published version of @tleed5/foo 

## Notes
* Lerna is set to independent versioning, meaning that each package can have a different version to each other. 