![Logo](../images/tedder.png)

> **Tedder - a scrum git branch manager**

## Background

In my team we have been adopting scrum to manage work items. We set a sprint to be one week length and we have a git sprint branch which is used to release the work done in the sprint. 

## Why

- We do have a naming standard for the sprint branch but it's sometimes violated for some reasons, leading to inconsistent branches. Our naming standard is __feature/[yyyy][mm][dd]__, therefore the branch should be __feature/20180809__ for the sprint ending in 9th Aug 2018. I do see these kinds of violations:
    - feature/0809      - missing year
    - feature/180809    - short year
    - featuer/20180809  - tries to follow the standard but makes a typo
      
- If more than one developer participates in the sprint, they usually need to hang around asking whether the branch has been created since only one branch needs creating, leading to unnecessary communication and development interruption.

## How

- For detail usage, refers to the [docs](https://github.com/n0ruSh/tedder)
- Basically you would want to setup a config file in your git repo:

``` 
// .tedderrc
{
    "day": "Thu",
    "template": "feat/[yyyy][mm][dd]"
}

```

Then when you run `tedder` command in your repo, it will compute the branch name based on your specificed template and day. In our exmaple above, it will be __feat/[yyyy][mm][dd]__ with __yyyy__, 
__mm__ and __dd__ being substituted with the full year, month and date of next Thursday. It will then check the corresponding remote branch exists or not. If the corresponding remote branch exists, meaning others have created the branch before, it will simple fetch the remote branch and switch to the branch. Otherwiese, it will automatically create the branch for you and push it to remote repo.

You can also set a script in your package.json to use it locally.

```
//package.json

{
    script: {
        "scrum": "tedder"
    }
}

```

Then you can simply do

```
npm run scrum
```

- You can also use it globally from command line with options.

```
tedder -t 'hotfix-[yy][mm][dd]' -d Mon -n 1
```

This will setup the hotfix branch for next Monday for you.