# GladStoriesEngine (GSE) Editor & Runtime

This is a new interactive fiction engine made for the specific purposes:

- Have a human readable JSON
- Make it possible to edit the interactive story structure in any editor. Even in Notepad and TextEdit.
- Only basic features are provided: branching and image support.
- One JSON file contains story used for editor AND the saved progress of the player.
- Run Editor + Runtime on Smartphones.

## Online editor&runtime: [Loca Deserta Online](https://locadeserta.com/index_en.html)

## Human Readable JSON Data Structure

The format is intentionally made to be as human eye friendly as possible. Other interactive fiction formats include tons of features, custom scripting, if-else variables, inventory stuff or even have a custom Domain Specific Language (DSL).

Creating DSL is not a problem but it requires some initial effort to use it and understand. In DSL cases, you are starting learning a new programming language which is very poorly designed. Also, the tooling support for such DSLs is poor or non-existent. It requires a custom editor to be used in the web or on the desktop. Mobile support for other engines does not exist at all.

#### GSE format mimics the normal printed book
It has a title, description, authors properties. And it has a first Page, called **root**. When you read the printed book you do it sentence by sentence and passage by passage, then page by page. Ocasionally, you may see some pictures printed in the book.

The GSE format does absolutely the same: **root** page is where you open the book. Then you read passage by passage, in GSE it is called **node**. Each node may contain image, associated with action described in the text. When you finish the Page, you have to make a decision (if you are still alive in the story ;) ). These decisions are described in **next** array. Each decision has a text, which is shown to the user. And it has a complete next page, which is opened when user selected the option. Then the process of parsing the story continues recursively! User reads the passages of next page, makes **next** choice and then continues reading the referenced **next** Page.

#### The **GladStoriesEngine** is a specific JSON format

```json
{
  "title": "After the Battle",
  "description": "At the beginning of the XVII century...",
  "authors": "Dmytro Gladkyi, Someone else",
  "year": 1628,
  "root": {
    "endType": null,
    "nodes": [
            {
                "text": "Dmytro lay hidden in the thicket far from the water",
                "imageType": "river"
            },
            {
                "text": "The Cossack lay like this for a long time."
            }
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
- **year** - a year as an integer. Usually tells in what year the story has happened.

## Root
**root** is a json of type [**Page**](#Page).

### Page
- **endType** - defines whether the main hero dies or continues to live when this page is finished. Might be null when there are next pages available. *Currently, GSE supports only "ALIVE" or "DEAD" type ends.*
- **nodes** - list of [**PageNode**](#PageNode). Contains passages which are shown one-by-one to the user. In canonical runtime user is required to press "Continue" button to get to the next passage.
- **next** - list of [**PageNext**](#PageNext). Contains a list of options available for the user when the page ends.

### PageNode
- **text** is required and contains a String.
- **imageType** is optional and can have different predefined values: 

```
forest
boat
bulrush
camp
cossacks
river
landing
steppe
```

### PageNext
- **text** - a String. Contains the text of the option in selection, which user can select.
- **nextPage** - a [**Page**](#Page).

The processing of the JSON is recursive. Every time user selects next option, the runtime replaces currentPage with the [nextPage](#PageNext) and the process continues until the [endType or the Page](#Page) is not null.

## JSON Schema validators
This repo contains [gladstory json schema](./gladstory.schema.json). It can be used to verify your JSON structure. The schema contains multiple types which are referenced inside the same schema.

There is an example of the story saved with the state info:
[example_story_with_history.json](./example_story_with_history.json).

You can use online tool [https://www.jsonschemavalidator.net](https://www.jsonschemavalidator.net) for validations. It supports $ref fields used in the schema.

# Implementations

## Dart

[There is a pub.dev package written in Dart](https://pub.dev/packages/gladstoriesengine). It supports all the features defined above. Follow the example and README.md of the package.

## Smalltalk

The effort to port this engine to the Smalltalk (Pharo) has started and is located here: https://github.com/gladimdim/gladstoriessmalltalk
