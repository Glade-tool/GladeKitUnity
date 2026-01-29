# Migration Guide: Converting .unitypackage to UPM Package

This guide explains how to extract the contents from `GladeKit.unitypackage` and organize them into the UPM package structure.

## Current Structure

The package is now set up with the following UPM-compatible structure:

```
GladeKitUnity/
├── package.json              # Package manifest (already created)
├── Runtime/                  # Runtime scripts and assets go here
│   └── GladeKit.Runtime.asmdef
├── Editor/                   # Editor-only scripts and assets go here
│   └── GladeKit.Editor.asmdef
├── Samples~/                 # Optional sample scenes (optional folder)
├── README.md
├── EULA.md
└── LICENSE-THIRD-PARTY.md
```

## Steps to Extract and Organize Content

### Option 1: Using Unity (Recommended)

1. **Create a temporary Unity project** (or use an existing one)

2. **Import the .unitypackage**:
   - In Unity, go to `Assets → Import Package → Custom Package...`
   - Select `GladeKit.unitypackage`
   - Click "Import" and import all assets

3. **Locate the imported assets**:
   - The assets will be imported into your `Assets/` folder
   - Look for folders like `GladeKit`, `GladeAgenticAI`, or similar

4. **Organize the assets**:
   - **Runtime scripts** (scripts that run in builds): Copy to `Runtime/` folder
   - **Editor scripts** (scripts with `#if UNITY_EDITOR` or in Editor folders): Copy to `Editor/` folder
   - **Other assets** (prefabs, materials, textures, etc.): Place in `Runtime/` or appropriate subfolder

5. **Update namespaces** (if needed):
   - Ensure all scripts use the `GladeKit` namespace (or update the asmdef files)
   - Update any `using` statements that reference old paths

### Option 2: Manual Extraction (Advanced)

If you have access to the original source files, you can manually copy them:

1. Copy runtime C# scripts → `Runtime/`
2. Copy editor C# scripts → `Editor/`
3. Copy other assets (prefabs, materials, etc.) → `Runtime/` or appropriate subfolders
4. Ensure all `.meta` files are preserved (Unity will regenerate them if needed)

## Important Notes

- **Assembly Definitions**: The `.asmdef` files are already created. If your scripts reference other assemblies, update the `references` array in the asmdef files.

- **Editor Folder**: Any scripts in the `Editor/` folder will only compile in the Unity Editor, not in builds.

- **Meta Files**: Unity will automatically generate `.meta` files for assets. You can commit these to git.

- **Package.json**: Update the `repository.url` field in `package.json` with your actual git repository URL.

## Testing the Package

1. **Commit and push** your changes to git
2. **Test installation** in a Unity project:
   - Open Unity Package Manager (`Window → Package Manager`)
   - Click `+` → `Add package from git URL`
   - Enter your git URL: `https://github.com/yourusername/GladeKitUnity.git`
3. **Verify** that the package installs correctly and all scripts compile

## Next Steps

After organizing the content:

1. Update `package.json` with the correct repository URL
2. Test the package installation
3. Update version numbers as you release updates
4. Consider adding samples to the `Samples~/` folder
