See this template in action [here](https://hannesdelbeke.github.io/wiki/)

TODO step by step guide on how to setup.
which settings to configure in the config file.
folder layout
branch infrastructure

NOTES:
- if you create a file index.md in a folder, that page will open when clicking on the folder in the side bar.

TODO:
- fork/make from template
- copy your vault notes in notes folder (don't change the foldername)
- token?
- in your notes folder, create a file index.md. This is the first page people see on your website.
- do not make a folder named "assets", or a folder with the same name as your repo e.g. "wiki_templates"

## Features
### Done
- [x] [[wikilinks]]
- [x] backlink support, forked Andy's [note-link-janitor](https://github.com/andymatuschak/note-link-janitor) and added a few fixes
- [x] [obsidian graph](https://hannesdelbeke.github.io/wiki_template/Graph%F0%9F%95%B8%EF%B8%8F/)
- [x] dark/light theme
- [x] every page has a "edit on github" pencil in top right, which links to the page on github
- [x] hashtag
- [x] dynamic content bar
- [x] table of content (page)
- [x] various fancy markdown syntax, e.g. LaTeX, mermaid, etc.
- [x] configure landing page / start page. index.md file in notes
### TODO
- [ ] hashtag search click
- [ ] preview [hover link](https://github.com/ObsidianPublisher/tooltips-internal-link)
- [ ] submodule support for actions **TODO**
- [ ] currently action doesn't work if no image or notes folder
- [ ] handle limitation. do not make a folder in notes with the reponame

## contribute
TODO local run site instructions.

## license
currently AGPL, and has to confirm to this when redistributed. (easiest is to just keep AGPL)
This is because some python hooks were used from [obsidian-mkdocs-publisher-template](https://github.com/ObsidianPublisher/obsidian-mkdocs-publisher-template)
