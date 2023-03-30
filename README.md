
> [!ERROR]
> If you see this warning in Obsidian you open the wrong folder.
> Open the notes folder, not the folder you cloned the repo in


This repo hosts [https://techartorg.github.io/TAO-Wiki/](https://techartorg.github.io/TAO-Wiki/)

This README is not included in the wiki, everything in the notes folder is included in the wiki.

### Guidelines for editing
- don't use colons in filenames., alphanumerical _- only 🙏
- if linking/referencing other notes, use wikilinks and edit with Obsidian if possible.

### Simple in browser edit
- To update this wiki, simply edit any .md files in the notes folder.
- After being merged in the `main` branch an action will auto update the github-page
- The github page can be found on [https://techartorg.github.io/TAO-Wiki/](https://techartorg.github.io/TAO-Wiki/)

### Advanced edit with Obsidian
⚠️ Obsidian settings & installed plugins are committed, carefull when editing them ⚠️ 

1. fork and clone this repo
2. install [Obsidian](https://obsidian.md/)
3. Open the repo folder with obsidian.
4. make edits like usual, any updates should be auto pushed to your fork
5. create a PR, after merge of your PR the github-page will auto update

### Tech
- we use mkdocs to auto convert markdown files to HTML
- we use github actions to automate the build process. 
- any commit to the `main` branch triggers a copy of the notes from `main` to `mkdocs` branch
- any commit in the `mkdocs` branch triggers a mkdocs build, to auto convert the markdown to html, using the settings in `mkdocs.yml`
- html output is saved in the `gh-pages` branch. this is where all the HTML stuff like css files etc live. (right now the build HTML files aren't actually committed)
- the files from the `gh-pages` branch are then automatically uploaded to github-pages by the github action.

You can easily set this up yourself with the [wiki-template](https://github.com/hannesdelbeke/wiki_template) from [@hannesdelbeke](https://github.com/hannesdelbeke)

## license
Github actions and site build code is currently AGPL, because some AGPL python hooks were used from [obsidian-mkdocs-publisher-template](https://github.com/ObsidianPublisher/obsidian-mkdocs-publisher-template)
