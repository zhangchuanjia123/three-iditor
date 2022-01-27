Writing unit tests for undo-redo commands
===

### Overview ###

Writing unit tests for undo/redo commands is easy.
The main idea to simulate a scene, execute actions and perform undo and redo.
Following steps are required.

1. Create a new unit test file
2. Include the new command and the unit test file in the editor's test suite
3. Write the test
4. Execute the test

Each of the listed steps will now be described in detail.

### 1. Create a new unit test file ###

Create a new file in path `test/unit/editor/TestDoSomethingCommand.js`.

### 2. Include the new command in the editor test suite ###

Navigate to the editor test suite `test/unit/unittests_editor.html` and open it.
Within the file, go to the `<!-- command object classes -->` and include the new command:

```html
// <!-- command object classes -->
//...
<script src="../../editor/js/commands/AddScriptCommand.js"></script>
<script src="../../editor/js/commands/DoSomethingCommand.js"></script>         // add this line
<script src="../../editor/js/commands/MoveObjectCommand.js"></script>
//...
```

It is recommended to keep the script inclusions in alphabetical order, if possible.

Next, in the same file, go to `<!-- Undo-Redo tests -->` and include the test file for the new command:

```html
// <!-- Undo-Redo tests -->
//...
<script src="editor/TestAddScriptCommand.js"></script>
<script src="editor/TestDoSomethingCommand.js"></script>              // add this line
<script src="editor/TestMoveObjectCommand.js"></script>
//...
```

Again, keeping the alphabetical order is recommended.

### 3. Write the test ###

#### Template ####

Open the unit test file `test/unit/editor/TestDoSomethingCommand.js` and paste following code:

```javascript
module( "DoSomethingCommand" );

test("Test DoSomethingCommand (Undo and Redo)", function() {

    var editor = new Editor();

    var box = aBox( 'Name your box' );

    // other available objects from "CommonUtilities.js"
    // var sphere = aSphere( 'Name your sphere' );
    // var pointLight = aPointLight( 'Name your pointLight' );
    // var perspectiveCamera = aPerspectiveCamera( 'Name your perspectiveCamera' );

    // in most cases you'll need to add the object to work with
    editor.execute( new AddObjectCommand( editor, box ) );


    // your test begins here...


} );
```

预定义代码只是为了简化开发，您不必坚持使用它。
但是，该测试应该包含至少一个`editor.execute()`, 一个 `editor.undo()` 和一个 `editor.redo()` 调用.

Best practice is to call `editor.execute( new DoSomethingCommand( {custom parameters} ) )` **twice**. Since you'll have to do one undo (go one step back), it is recommended to have a custom state for comparison. Try to avoid assertions `ok()` against default values.
However, the test should cover at least one `editor.execute()`, one `editor.undo()` and one `editor.redo()` call.

#### 断言 ####
在两次执行 `editor.execute()` 之后，您可以执行第一次断言，以检查执行是否正确。

Next, you perform `editor.undo()` 并检查最后一个操作是否被撤消。

最后，执行`editor.redo() `并验证值是否符合预期。

### 4.执行测试t ###

在浏览器中打开编辑器的单元测试套件`test/unit/unittests_editor.html` 检查测试框架的结果。
