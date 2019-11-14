# GladStoriesEngine (GSE) Editor & Runtime

This is a new interactive fiction engine made for the specific purposes:

- Have a human readable JSON
- Make it possible to edit the interactive story structure in any editor. Even in Notepad and TextEdit.
- Only basic features are provided: branching and image support.
- One JSON file contains story used for editor AND the saved progress of the player.
- Runs on Smartphones.

## Human Readable JSON Data Structure

The format is intentionally made to be as human eye friendly as possible. Other interactive fiction formats include tons of features, custom scripting, if-else variables, inventory stuff or even have a custom Domain Specific Language (DSL).

Creating DSL is not a problem but it requires some initial effort to use it and understand. In DSL cases, you are starting learning a new programming language which is very poorly designed. Also, the tooling support for such DSLs is poor or non-existent. It requires a custom editor to be used in the web or on the desktop. Mobile support for other engines does not exist at all.

#### GSE format mimics the normal printed book
It has a title, description, authors properties. And it has a first Page, called **root**. When you read the printed book you do it sentence by sentence and passage by passage, then page by page. Ocasionally, you may see some pictures printed in the book.

The GSE format does absolutely the same: **root** page is where you open the book. Then you read passage by passage, in GSE it is called **node**. Each node may contain image, associate with action described in the text. When you finish the Page, you have to make a decision (if you are still alive in the story ;) ). These decisions are described in **next** array. Each decision has a text, which is shown to the user. And it has a complete next page, which is opened when user selected the option. Then the process of parsing the story continues recursively! User reads the passages of next page, makes **next** choice and then continues reading the referenced **next** Page.

#### The **GladStoriesEngine** is a specific JSON format

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
    "next": [
      {
        "text": "Shoot the rifle",
        "nextPage": {}
      },
      {
        "text": "Run away",
         "nextPage": {}
      }
    ]
  }
}
```

This is the absolutely minimum data required to make a valid **GSE** JSON file which is suitable for editor and runtime.

### Field by field annotation:

- **title** - the title of the story
- **description** - the summary of the story
- **authors** - list of authors separated by commas
- **root** - the first Page of the story which contains a Tree-like multi-leaf structure.

**root** is a Page. It consists following fields:
- **endType** - defines whether the main hero dies or continues to live when this page is finished. Might be null when there are next pages available. *Currently, GSE supports only "ALIVE" or "DEAD" type ends.*
- **nodes** - contains passages which are shown one-by-one to the user. In canonical runtime user is required to press "Continue" button to get to the next passage.
- **next** - contains a list of options available for the user, when the page ends.

Each **node** 

TBC

