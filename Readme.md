# Sir Trevor JS

_Intro Needed_

A rebuild of the ITV news editor, with the Javascript pulled out of Ruby gem and separated into it's own project for easy use with any server-side language. 

## Features

- Don't store any HTML in database
- Flexible and intuitive interface
- Easy to extend / customise
- Drag and drop block re-ordering

## Quick Start

Grab the `sir-trevor.js` or `sir-trevor.min.js` file from the `lib` directory. This is the core Sir Trevor instance with the default Formatters and BlockTypes.

Include a reference to `sir-trevor.css` in your head, located in the `lib` folder. 

    <link rel="stylesheet" href="sir-trevor.css" type="text/css" media="screen" charset="utf-8">

Create an instance of SirTrevor.Editor as follows (always wrap it in a document ready block)

    <script>
      $(function(){
        new SirTrevor.Editor({
          el: $('.sir-trevor')
        });
      });
    </script>

Your HTML should look something like this:

    <form>
      <textarea class="sir-trevor"></textarea>
      <input type="submit">
    </form>

You can limit the types of Blocks in the editor by passing a `blockTypes` array through to the editor instance as follows:

    <script>
      $(function(){
        new SirTrevor.Editor({
          el: $('.sir-trevor'),
          blockTypes: ['TextBlock', 'BlockQuote'] // This instance will now only have these types available to it
        });
      });
    </script>

Your SirTrevor editor instance will be bound to the submission of it's parent form element. On submission of the form the editor will validate and then serialise all of the Blocks on the page, storing the resulting JSON into the `<textarea>` you provided. You can them do as you wish with this on your server-side processing. 
  
## Flow

### Loading data:

- Parse JSON
- Create new blocks from the JSON representation
- `render` each new block
- Call the `loadData` function on each block
- Call the `onBlockRender` method after render

### Submission of form

For each block within each `SirTrevor.Editor` instance:

- Run validation (calls default or overwritten `validate` function)
- If the validation passes, run the `toData` function (default or overwritten). `toData` will save the state of the block onto itself. 

Once all blocks have run through this sequence the complete JSON state is serialised onto the instances `$el`.  

## Structure 

### BlockTypes

The editor itself is made up of Blocks. Sir Trevor comes bundled with the following BlockTypes out of the box:

- Text
- Quote
- Unordered List
- Image (simple)
- Gallery

There are more blocks available in the dedicated Blocks repository, [available here](https://github.com/madebymany/sir-trevor-blocks) 

### Blocks

A Block will always belong to a BlockType and inherits some of the methods and properties of the BlockType. A Block will be rendered when the user selects a new BlockType from the marker bar, or when we're rendering the content from JSON. 

### Formatters

Rich text block types (Text, Quote, Lists etc) use an *ultra* simple formatting system. The formatters allow for the styling of the content in these blocks. Out of the box, Sir Trevor ships with the following formatters:

- Bold
- Italic
- Link
- Unlink

## Extending Sir Trevor



