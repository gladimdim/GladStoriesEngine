# GladStoriesEngine (GSE) Editor & Runtime

This is a new interactive fiction engine made for the specific purposes:

- Have a human readable JSON data structure
- Make it possible to edit the interactive story structure in any editor, even in Notepad and TextEdit.
- Only basic features are provided: branching and image support.
- One JSON file contains story used for editor AND the progress of the player.

## Human Readble JSON Data Structure

The format is intentionally made to be as human-eye friendly as possible. Other interactive-fiction formats include tons of features, custom scripting, if-else variables, inventory stuff or even have a custom Domain Specific Language (DSL).

Creating DSL is not a problem but it requires some initial effort to use it and understand. You are basically start learning a new programming language which is very poorly designed. Also usually the tooling support for such DSLs is always poor or non-existent. It requires a custom editor to be used in the web or on the desktop. Mobile support for other engines does not exist at all.

The **GladStoriesEngine** is just a specific JSON format:

```json
{
  "title": "After the Battle",
  "description": "At the beginning of the XVII century...",
  "authors": "Dmytro Gladkyi, Someone else",
  "root": {
    "endType": null,
    "nodes": [
            {
                "text": "Dmytro lay hidden in the thicket far from the water",
                "imageType": "river"
            },
            {
                "text": "The Cossack lay like this for a long time."
            },
    ],
    "next": []
  }
}
```

This is the absolutely minimum data required to make a valid **GSE** JSON file which is suitable for editor and runtime.

Field-by-field annotation:

- **title** - the title of the story
- **description** - the summary of the story
- **authors** - list of authors separated by commas
- **root** - the first page of the story which contains a Tree-like multi-leaf structure.

Root property consists following fields:
- **endType** - defines whether the main hero dies or continues to live when this page is finished. Might be null when there are next pages available. *Currently, GSE supports only "ALIVE" or "DEAD" type ends.*
- **nodes** - contains passages which are shown one-by-one to the user. In canonical runtime user is required to press "Continue" button to get to the next passage.
- **next** - contains a list of options available for the user, when the page ends.

Each **node** 

