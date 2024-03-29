# Exercises

You can clone all the exercises for this course into an ignored hidden folder called
`exercises`. All the exercises have their own git repos, so take care not to
accidentally add them to this repository; submodules and nested git projects are
messy.

## Clone everything

Do a little shell-dance to clone each of the exercises (check the map of
exercises, git urls, and where they'll end up with `cat exercise-map.txt`)

```sh
mkdir -p exercises
while read -r line; do
  project=`echo $line | cut -d ':' -f 1`
  mkdir -p $project
  pushd $project
  url=`echo $line | cut -d ':' -f 2,3`
  git clone $url .
  popd
done < ../list-of-exercises.txt
```

## Git pull all

To refresh all the exercises

From `exercises`:

```sh
find . -type d -maxdepth 2 -mindepth 2  |
while read -r project; do
        pushd $project
        echo $project
        git checkout main
        git pull
        popd
done
```

## Update the list of exercises

i.e. if you added or removed an exercise (folder within one of the weeks in
`exercises`, with a git remote)

from exercises:

```sh
setopt PUSHDSILENT
find . -type d -maxdepth 2 -mindepth 2  |
while read -r project; do
  pushd $project
  echo -n "$project:"; git remote get-url origin
  popd
done | sort > ../list-of-exercises.txt
```

## Add another remote to all the exercises and push

When we create a new github org to run the course, we need to create new repos, 
add them as new remotes, and push each project and exercise to the new repo.

* Edit so that the org name and remote name match the term
* Requires the [Github CLI](https://cli.github.com/)

from `exercises`:

```sh
find . -type d -maxdepth 2 -mindepth 2  |
while read -r project; do
        pushd $project
        echo $project
        git checkout main
        name=$(echo $project | cut -d '/' -f 3)
        gh repo create "frontend-web-dev-oct-23/$name" --source=. --private --remote=oct-23 --push 
        gh repo edit "frontend-web-dev-oct-23/$name" --template 
        popd
done
```

## For all exercises, push a branch to a given remote

change 'main' and 'oct-23' as appropriate

from `exercises`:

```sh
find . -type d -maxdepth 2 -mindepth 2  |
while read -r project; do
        pushd $project
        echo $project
        git checkout main
        git push oct-23 main
        popd
done
```

## Exercises 

After you run 'clone all' script, you should have a directory `exercises` with
the exercises in the `list-of-exercises.txt` file

`tree -L 2 exercises` shows the exercises.
