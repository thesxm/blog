TEMPLATE: base-post.html
TITLE: Creating a simple static file generator using python
TIMESTAMP: 1735621130000 

[TOC]

## Introduction
This post is about my project [housemd](https://github.com/thesxm/housemd), a very minimal and highly extensible static site generator which powers this blog. I will try to explain the working of it's features (as of [this commit](https://github.com/thesxm/housemd/tree/769f67ce94cbad636d5aa5cfef7b9a6b544f661b)) and possible future developments.

## Motivation
I love making things from scratch, so when I decidde to start a blog it naturally occured to me that I should write my own static site generator which I could then use in my blog, and so I created `housemd`, a markdown based static site generator written in python.

## Features
- Converts markdown to html using the `markdown` library, all the [extensions](https://python-markdown.github.io/extensions/) are enabled.
- Provides `metadatabase` feature which prvodes information about the generated sites to your templates.
- Configurable using a JSON Config file.
- Provides the `housemd-live` utility which comes in handy when locally testing your website. It spawns a web server from your output directory and listens to any file changes in the source or templates directory, rebuilding the website when triggered.

Read the [README.md](https://github.com/thesxm/housemd/blob/769f67ce94cbad636d5aa5cfef7b9a6b544f661b/README.md) for more information[^1].

[^1]: This file is from a specific commit, not from the latest version of `housemd`.

## Workings
### Metadatabase
In the config file you can optionally specify a metadatabase path under the `mdb` key as such:

```json
{
    "source": "source_dir",
    "output": "output_dir",
    "templates": "templates_dir",
    "mdb": "path/to/metadatabase.json"
}
```

When `housemd` translates a `.md` file to it's `.html` version, it takes it's metadata (provided by the `meta` extension) and stores it in the `metadatabase` dictionary. Once all the markdown files have been translated, the `metadatabase` dictionary is dumped to the specified path in JSON format.

When dealing with subdirectories in the source folder of the website, metadatabase converts the subdirectory names to keys and recursively stores dictionaries inside itself.

#### Example
If your source directory looks like this:

```
source_dir/
├── index.md
└── post/
    └── index.md
```

Where `source_dir/index.md` is:

```markdown
TEMPLATE: base.html
TITLE: Index File
```

and `source_dir/post/index.md` is:

```markdown
TEMPLATE: base-post.html
TITLE: Post Index
TIMESTAMP: 0
```

The output directory will be (assuming `mdb` is `out_dir/mdb.json` in the config file):

```
out_dir/
├── index.html
├── post/
|   └── index.html
└── mdb.json
```

Where `mdb.json` will contain:

```json
{
    "index.html": {
        "template": ["base.html"],
        "title": ["Index File"]
    },
    "post": {
        "index.html": {
            "template": ["base-post.html"],
            "title": ["Post Index"],
            "timestamp": ["0"]
        }
    }
}
```

This metadatabase file can be fetched using javascript in your templates and be used to implement features. Check [this](https://github.com/thesxm/blog/blob/e274bb20343a5d2e3d09927486dfd36431b4093f/templates/posts.html)[^2] out for an example, it is from the source code of this blog.

[^2]: This file is from a specific commit, not from the latest version of this blog.

### housemd-live
`housemd-live` is a command line utility which is used to develop and test the website. It uses `watchdogs` library to monitor file changes and automatically rebuilds the website when a change is detected in either the source or the templates directory. It also starts a simple http server from the output directory to run the website locally.

The part of `housemd-live` I had most fun implementing was handling file events related to temporary files created by some editors.

Basically, once I had implemented a basic version of `housemd-live` and tried to test it, I opened a file and changed it's content hoping to see `housemd-live` detect the change and rebuild the website, and it did, but it detected 5 events instead of 1 and thus rebuilt the site 5 times.

Turns out when you save a file, editors create, and very quickly delete, some temporary files to safely update the data of the original file, and `housemd-live` was picking up on the creation, moving, updating and deletion of those temporary files and rebuilding the website multiple times. To solve this issue I wrote a timer (as of right now, it is hardcoded to 3 seconds) which would start when the first change is detected and would reset for every new change detected within that timeframe. Once the timer runs without any interruptions (i.e. no new changes have been detected for 3 seconds) it rebuilds the website. You can find the messy code [here](https://github.com/thesxm/housemd/blob/769f67ce94cbad636d5aa5cfef7b9a6b544f661b/src/housemd/__main__.py)[^1].

## Future developments
I have a couple of things in mind for `housemd`. Some of which are:
- **`housemd-init`**: An init utility to quickly setup and run housemd.
- **Multithreading Support**: Use multithreading to increase build speed by parallely translating the files.
- **Optimizing and Testing**: Rewrite and optimize parts of `housemd`, improving the error handling and writing tests.

Feel free to contribute to the project!
