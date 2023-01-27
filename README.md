#  Auto Generate Conventional Commits with commitizen ,devmoji and husky for NodeJS :boom:

  


###  Hi! :alien:

  

In this **article**, I will discuss how to write awesome commits with conventional commits

**Using**

[  `commitizen `](https://www.npmjs.com/package/commitizen) , [`pnpm`](https://www.npmjs.com/package/pnpm) , [`cz-conventional-changelog`](https://www.npmjs.com/package/cz-conventional-changelog) , [`husky`](https://www.npmjs.com/package/husky) and [`devmoji`](https://www.npmjs.com/package/devmoji).

**🔀 Before start :**
`we need to understand what the conventional commits and why using it ?`

### What is conventional commits !
- It is a lightweight convention on top of **commit messages**. It provides an easy set of **rules** for creating an explicit commit history which makes it easier to write automated tools on top of.
 
### Why we Use Conventional Commits !
- Automatically generating CHANGELOGs.
- Automatically determining a semantic version bump (based on the types of commits landed).
- Communicating the nature of changes to teammates, the public, and other stakeholders.
- Triggering build and publish processes.
- Making it easier for people to contribute to your projects, by allowing them to explore a more structured commit history.
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

***To read more :***  [conventionalcommits](https://www.conventionalcommits.org/en/v1.0.0/)

******************************************************************

If you don't have **pnpm** :package:

:twisted_rightwards_arrows: you should **install** it :
```
npm install -g pnpm
```  

:twisted_rightwards_arrows: or using **npm** :package:



##  Content

 1. [Step #1 : Dealing with Commitizen package](#commitizen)

 2. [Step #2 : Commitizen Adapter]( #adapter)

 3. [Step #3 : Dealing with husky](#husky)

 4. [Step #4 : Applying emojis with commits](#emojis)

 5. [Results](#final)



  <a name="commitizen"></a>
##  Step #1 : Dealing with Commitizen package

:twisted_rightwards_arrows: we need to install **commitizen** inside the project (locally) or globally with pnpm package.


>  This example assumes that the project has been set up to **use Commitizen locally**

```
#locally
pnpm add -D commitizen
```
#####
```
#globally
pnpm add -g commitizen
```
#####
:white_check_mark: Create a `.czrc` file in your `home` directory :twisted_rightwards_arrows: in the **path** that will be refer to the globally or locally path installed **commitizen adapter**. 
```
# .czrc
{
  "path":""
}
```
➕ Add pnpm scripts in your **`package.json`** file with **git-cz** or **cz** pointing to the local version of Commitizen:
```
# package.json
"scripts": {
  "commit":"git-cz"
}
```
**:triangular_flag_on_post: Or Using**
```
pnpm pkg set scripts.commit="git-cz"
```
>  Installing and running Commitizen locally allows you to ensure that developers are running on the exact same version of Commitizen on all machines.



<a name="adapter"></a>
##  Step #2 : Commitizen Adapter 

:white_check_mark: install **cz-conventional-changelog** and integrate it with the commitizen config inside **.czrc** file.
```
#locally
pnpm add -D cz-conventional-changelog
```
######
```
#globally
pnpm add -g cz-conventional-changelog
```
:white_check_mark: update **.czrc** file with commitizen adapter 
```
{
  "path":"node_modules/cz-conventional-changelog"
}
```
>  Note : if you install the package global , you don't need to apply cz-conventional-changelog with node_modules
```
{
  "path": "cz-conventional-changelog"
}
```



<a name="husky"></a>
##  Step #3 : Dealing with husky :

>  Husky improves your commits 🐶
>  -- You can use it to **lint your commit messages**, **run tests**, **lint code**, etc... when you commit or push. Husky supports [all Git hooks](https://git-scm.com/docs/githooks).

:white_check_mark: we will use it to apply emoijs with commits :smile:

####  :triangular_flag_on_post: let's start  
```
# install husky locally
pnpm add -D husky
```
####

```
# update pakage.json file
# run these commands
> pnpm pkg set scripts.prepare="husky install"
> pnpm prepare
```
<a name="emojis"></a>
##  Step #4 : Applying emojis with commits ✨

####  :white_check_mark: Install devmoji package
```
pnpm add -D devmoji
```
####  :white_check_mark: Add commit-msg hook
 ```
pnpx husky add .husky/commit-msg "head -1 $1 | pnpx devmoji --commit > $1"
```
**:triangular_flag_on_post: Note** :

-  `head -1 $1` is the same with `head -n 1 $1` to select the first line in the commit msg file

-  **- -commit :** is **true** by default , you can ignore it

-  also you can use `pnpx devmoji -e` in commit-msg hook to read last commit from `.git/COMMIT_EDITMSG` in the git root.

-  if **devmoji** is installed globally you don't need to add `pnpx`

  
###  ➕ Add some validations as a pro 🚀

  
```
# update commit-msg hook
# validation with regex
if ! head -1 $1 | grep -E "^(feat|fix|chore|docs|test|style|refactor|perf|build|ci|revert)(\(.+?\))?: .{1,}$"; then
echo "Aborting commit with invalid message." >&2
exit 1
fi
# message length validation
if ! head -1 $1 | grep -E "^.{1,88}$"; then
echo "Aborting commit with too long message." >&2
exit 1
fi
pnpx devmoji -e
# Or using
# head -1 $1 | pnpx devmoji --commit > $1
```

  
<a name="final"></a>
##  :bookmark: Results:
```
# adds a change in the working directory to the staging area
git add .
# Run this command
pnpm commit
```
####  There are some questions to follow :arrow_down:

-- ❓ Select the type of change that you're committing: `(Use arrow keys)`



|  |  |
|------------|----------|
| feat:      | A new feature |
| fix: | A bug fix |
| docs: | Documentation only changes
style: | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) |
| refactor: | A code change that neither fixes a bug nor adds a feature |
| perf: | A code change that improves performance |
| test: | Adding missing tests or correcting existing tests |
| build: | Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm) |
| ci: | Changes to our CI configuration files and scripts Travis, Circle, BrowserStack, SauceLabs) |
| chore: | Other changes that don't modify src or test files |
| revert: | Reverts a previous commit |

  
-- ❓ What is the scope of this change (e.g. component or file name):`(press enter to skip)`

-- ❓ Write a short, imperative tense description of the change `(max 94 chars)`:

-- ❓ Provide a longer description of the change: `(press enter to skip)`

-- ❓ Are there any breaking changes? `(y/N)`

-- ❓ Does this change affect any open issues? `(y/N)`