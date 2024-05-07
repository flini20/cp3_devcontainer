# Cheat Sheet to setup environment for cellpose3

_by Raphael Feurstein - raphael@feurstein-solutions.at_
[Youtube Video](https://www.youtube.com/watch?v=SDa3v4Quj7Y)

## Step 1: Install VSCode

- For Windows: [Windows Documentation](https://code.visualstudio.com/docs/setup/windows)
- For Linux: [Linux Documentation](https://code.visualstudio.com/docs/setup/linux)
- For macOS: [macOS Documentation](https://code.visualstudio.com/docs/setup/mac)

## Step 2: Install Docker Desktop

_Skip this step if you are on a **Linux** computer!_

- For Windows: [Windows Documentation](https://docs.docker.com/desktop/install/windows-install/)
- For macOS: [macOS Documentation](https://docs.docker.com/desktop/install/mac-install/)

## Step 3: Install VSCode Extensions

1. Open the side panel in VSCode an go to the _extensions_ tab
2. Search for the _Remote Development_ extension from Microsoft (It is a pack of more extensions)
3. Install the _Remote Development_ extension

## Step 4: To be continued

# _For Creation of DevContainer_

## Step 1: Follow Step 1 to 3

## Step 2: Create new GitHub repository

Create repository with following conditions on GitHub:

1. Add a README file
2. Add .gitignore for Node
3. Optional: Add MIT license

## Step 3: Clone Repository in Container Volume

1. Open VSCode
2. Press Crl + Shift + P (Cmd + Shift + P on macOS) -> It opens the Command Palette
3. Enter `>dev containers: clone repository in container volume` -> This will clone the GitHub repository into the new created container
4. Select _Clone a repository from GitHub in a Container Volume_
5. Select the in Step 2 created repository on GitHub (alternatively you can enter the link to your GitHub repository, if the repository isn't shown)
6. Select the main branch (by default _main_)
7. Wait until the dev container is build up (1/2)
8. You will be asked for _Add Dev Container Configuration Files_ - Select _Show All Definitions..._
9. Select _Default Linux Universal_ -> It is used also in GitHub Codespaces
10. Ignore the question after additional features and click on _OK_
11. Wait until the dev container is build up (2/2) (it takes quite long)

## Step 4: Configure Container

1. Place the needed Dockerfile in the folder _.devcontainer_
2. Open the file _devcontainer.json_ in the same folder _.devcontainer_
3. Replace the "image" code with following code:
`	"build": {
			// Path is relative to the devcontainer.json file.
			"dockerfile": "Dockerfile",
			// Update 'args' to set an argument.
			// "args": { }
	},`
4. Rename the Devcontainer as needed behind the `"name":` Code
5. Push to GitHub with Terminal:
`git add.
git commit -m "add Dockerfile, devcontainer configured for Dockerfile"
git push`

## Step 5: Rebuild Container

1. Press Crl + Shift + P (Cmd + Shift + P on macOS)
2. Enter `>dev containers: rebuild container`

