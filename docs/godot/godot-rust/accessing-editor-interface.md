# Accessing the Editor Interface

If done incorrectly, this will lead to a segmentation fault with no way to debug without more advanced debugging tools. What is crucial is to ensure your `EditorPlugin` class is the only class using `EditorInterface` and that it is configured for Godot-Rust to recognize it as an editor plugin!

## Full Example

``` rust
#[derive(GodotClass)]
#[class(tool, editor_plugin, base=EditorPlugin)]
struct MyEditorPlugin {
    #[base]
    base: Base<EditorPlugin>,
}


#[godot_api]
impl IEditorPlugin for MyEditorPlugin {
    fn enter_tree(&mut self) {
	    let Some(editor) = self.base.get_editor_interface() else {
            return;
        };
        // then we can use "editor" to mess with the editor interface 
    }

    fn exit_tree(&mut self) {}
}



```

> Crucially, the `editor_plugin` part of the class definition is what determines to godot-rust that this class is editor-only. This will prevent the errors where EditorInterface is not available.

## Functionality

This procudure creates essentially a plugin like you could create with GDScript. All of that same functionality is exposed through `self.base`. However, given that this is compiled at a lower level, you can also hook into other engine features that are less accesible to GDScript. Additionally, this method does not allow disabling the plugin because the entire GDExtension is either successfully loaded or it isn't!
