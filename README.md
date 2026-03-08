<div align="center" style="display: flex; justify-content: center; align-items: center; gap: 20px;">
  <img src=https://rojo.space/assets/images/splash-a50c3fba664e79034eb6e98505a993b1.png />
</div>

<br>

<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
  <img src="https://i.imgur.com/KAHYaxx.png" style="max-height: 75px;" />

  <h1>Rojo Luau</h1>
</div>

## About

>[!IMPORTANT]
> You MUST have Lune setup in your current project to use this wrapper.

### Installation
> Create a folder to hold the wrapper.
```sh
mkdir ext
cd ext
```

> Create a file for the wrapper.
```sh
ni rojo.luau -type file
```

> Paste the contents from src/rojo.luau

*Done!*

## Path Specification

When you specify a path to either a script or directory, you should put @ as the first character of the string assuming its in your src directory. This allows for Rojo Luau to create two file versions, one for your build (optional) and one for your default file this is useful for things such as darklua.

## Project Example

Here is an example file which uses rojo luau.
The following code will create two files when built, build.project.json and default.project.json.

> rojo-project.luau
```lua
  --!strict
local rojo = require('@ext/rojo')

local create = rojo.create

return rojo.project {
	name = 'Rojo Project',

  -- Replaces the @ in file/directory names
	source_directory = 'src',

  -- For things like darklua where you need a seperate project json for builds
	compiled_directory = 'out', 

	tree = {
		ReplicatedStorage = create 'ReplicatedStorage' {
			client = '@client',
			shared = '@shared',
			packages = 'Packages',
		},

		ServerScriptService = create 'ServerScriptService' {
			server = '@server',
		},

		StarterPlayer = create 'StarterPlayer' {
			StarterPlayerScripts = create 'StarterPlayerScripts' {
				runtime = '@runtime.client.luau',
			},
		},
	},
}
```

Here's a Lune script which runs the project builder:

> .lune/build_sync_tree.luau
```lua
local project = require('../rojo-project')
local rojo = require('@ext/rojo')

rojo.build(project)
```