# Phase 1 Tool Test Prompts

## Migration count

- **Migrated (registry/ITool):** 28 tools  
- **Remaining in ToolExecutor switch:** 74 tools  
- **Total tool cases in switch:** 102  

Use the prompts below in the Glade AI chat to verify each migrated tool. Run at least one prompt per tool to confirm functionality is preserved.

---

## Batch 1: Create / transform / hierarchy

### create_primitive
- Create a cube in the scene.
- Add a sphere named "Planet".
- Create a Capsule called "Pillar" and a Quad called "Floor".

### create_game_object
- Create an empty GameObject named "Container".
- Create an empty GameObject named "SpawnPoint" as a child of Container.

### set_transform / set_local_transform
- Move the Cube to position 5, 0, 0.
- Set the local position of "Box" to 0, 1, 0.

### destroy_game_object
- Delete the GameObject named "Floor".
- Destroy "Box".

### set_game_object_active
- Hide "Planet" (set active to false), then show it again (set active to true).

### set_game_object_parent
- Make "Planet" a child of "Container".
- Unparent "Planet" (set parent to root).

---

## Batch 2: Think / scripts / selection / hierarchy

### think
- Think: "I will create a cube next."

### get_script_content
- Read the contents of Assets/Editor/GladeAgenticAI/Tools/ITool.cs.

### find_scripts / search_scripts
- Find all scripts whose name contains "Tool".
- Search script contents for the word "ExecuteTool".

### get_selection / set_selection
- What is currently selected in the Hierarchy?
- Select the GameObject named "Stage".

### list_children / rename_game_object / duplicate_game_object
- List the children of "Stage".
- Rename "Platform" to "MainPlatform".
- Duplicate the GameObject "Platform".

---

## Batch 3: Layer / tag / info / assets

### set_layer / set_tag
- Set the layer of "Platform" to "Default".
- Set the tag of "Platform" to "Untagged".

### get_gameobject_info / check_asset_exists / list_materials
- Get info for the GameObject "Stage".
- Check if Assets/Editor/GladeAgenticAI/Tools/ITool.cs exists.
- List all materials in the project (or with searchPattern "Lit").

---

## Batch 4: Folder / scene / compilation (NEW)

### create_folder
- Create a folder at Assets/TestFolder.
- Create a folder at Assets/Scenes/MyScenes (creates hierarchy).
- Create the same folder again (should report "Folder already exists").

### open_scene
- Open the scene at Assets/Editor/GladeAgenticAI/DemoScene/DemoScene.unity (or any existing .unity path in the project).
- Open scene Assets/Scenes/MainScene (add .unity if needed).

### save_scene
- Save the current scene.

### save_scene_as
- Save the current scene as Assets/Scenes/BackupScene.unity (creates Scenes folder if needed).

### refresh_asset_database
- Refresh the Asset Database.

### compile_scripts
- Trigger script compilation. (Tool returns success but skips actual compile to avoid domain reload loops.)

### get_script_compile_errors
- Get the current script compile errors. (May return "not supported" message.)

---

## One-shot sanity check (Batch 4)

1. Create a folder Assets/Phase1Test.
2. Create a folder Assets/Phase1Test/Scenes.
3. Save the current scene as Assets/Phase1Test/Scenes/SanityCheck.unity.
4. Refresh the Asset Database.
5. Ask: "What script compile errors are there?" (get_script_compile_errors).
6. Ask: "Compile scripts." (compile_scripts – should return success/skipped).

After step 6 you should have Assets/Phase1Test/Scenes/SanityCheck.unity (or the current scene path), and both refresh and compile_scripts should have returned success.

---

## Full regression (all batches)

Run a short flow that touches one tool from each batch:

1. Create a cube named "RegressTest".
2. Create an empty GameObject "Parent".
3. Set the position of RegressTest to 1, 0, 0.
4. Make RegressTest a child of Parent.
5. Think: "Regression step 5."
6. List the children of Parent.
7. Get selection (select RegressTest first if needed).
8. Set the layer of RegressTest to Default.
9. Get gameobject info for Parent.
10. Check if Assets/Editor/GladeAgenticAI/Tools/ITool.cs exists.
11. List materials with maxResults 5.
12. Create folder Assets/RegressionTestFolder.
13. Save the current scene.
14. Refresh the Asset Database.
15. Compile scripts.

If all steps succeed without errors, migrated tools are behaving as expected.
