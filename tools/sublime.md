## sublime

```
{
	"color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
	"file_exclude_patterns":
	[
		"*.lock",
		"package-lock.json"
	],
	"folder_exclude_patterns":
	[
		".svn",
		".git",
		".hg",
		"CVS",
		"node_modules",
		"dist",
		"static"
	],
	"font_size": 14,
	"ignored_packages":
	[
		"Vintage"
	],
	"open_files_in_new_window": false,
	"tab_size": 2,
	"word_wrap": "true"
}

```

```
{
	"bootstrapped": true,
	"channels":
	[
		"/Users/huyong/channel_v3.json"
	],
	"debug": true,
	"downloader_precedence":
	{
		"linux":
		[
			"curl",
			"urllib",
			"wget"
		],
		"osx":
		[
			"curl",
			"urllib"
		],
		"windows":
		[
			"wininet"
		]
	},
	"in_process_packages":
	[
	],
	"installed_packages":
	[
		"Alignment",
		"All Autocomplete",
		"AngularJS",
		"Babel",
		"BracketHighlighter",
		"ChineseLocalizations",
		"CTags",
		"Codecs33",
		"ColorHighlight",
		"ColorPick",
		"ColorPicker",
		"Colorsublime",
		"ConvertToUTF8",
		"CSS3",
		"Cssnext",
		"DocBlockr",
		"Emmet",
		"ESLint",
		"FileDiffs",
		"Git",
		"Git blame",
		"GitGutter",
		"HTML-CSS-JS Prettify",
		"jQuery",
		"JsFormat",
		"JSONLint",
		"JsPrettier",
		"LESS",
		"MarkdownEditing",
		"MarkdownPreview",
		"Minify",
		"MultiEditUtils",
		"Naomi",
		"Node Completions",
		"Package Control",
		"Pretty JSON",
		"React IDE",
		"Sass",
		"SideBarEnhancements",
		"Stylus",
		"SublimeCodeIntel",
		"SublimeLinter",
		"SublimeLinter-eslint",
		"Syntax Highlighting for PostCSS",
		"Tag",
		"TypeScript",
		"TypescriptCompletion",
		"Vue Syntax Highlight",
		"Vuejs Snippets"
	]
}

```


1. There are no packages available for installation

获取 http://packagecontrol.io/channel_v3.json，存放到本地 /Users/huyong/channel_v3.json

打开Preferences -> Package Settings -> Package Control -> Settings-User

添加

	"channels":
	[
		"/Users/huyong/channel_v3.json"
	],


2. Unable to download XXX. Please view the console for more details

打开Preferences -> Package Settings -> Package Control -> Settings-User

添加

	"debug": true,
	"downloader_precedence":
	{
		"linux":
		[
			"curl",
			"urllib",
			"wget"
		],
		"osx":
		[
			"curl",
			"urllib"
		],
		"windows":
		[
			"wininet"
		]
	},


cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/

git clone git://github.com/benmatselby/sublime-phpcs.git Phpcs




	{
		"color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
		"font_size": 15,
		"ignored_packages":
		[
			"Vintage"
		],
		"open_files_in_new_window": false,
		"tab_size": 2,
		"word_wrap": "true",
		"folder_exclude_patterns": [".svn", ".git", ".hg", "CVS", "node_modules","dist","static"],
		"file_exclude_patterns": ["*.lock","package-lock.json","package.json"],
	}























