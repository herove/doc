# Copyright

As a open source project, there must be a LICENSE file to claim the rights. 

Here are two examples of using [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0) and MIT licenses.

### Apache License, Version 2.0

This license requires to put following content at the beginning of every file:

```
// Copyright [yyyy] [name of copyright owner]
//
// Licensed under the Apache License, Version 2.0 (the "License"): you may
// not use this file except in compliance with the License. You may obtain
// a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
// WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
// License for the specific language governing permissions and limitations
// under the License.
```

Replace `[yyyy]` with the year of creation of the file. Then use personal name for personal project, or organization name for team project to replace `[name of copyright owner]`.

### MIT License

This license requires to put following content at the beginning of every file:

```
// Copyright [yyyy] [name of copyright owner]. All rights reserved.
// Use of this source code is governed by a MIT-style
// license that can be found in the LICENSE file.
```

Replace `[yyyy]` with the year of creation of the file.

### Other Notes

- Other types of license can follow the template of above two examples.
- If a file has been modified by different individuals/organizations, and two licenses are compatiable, then change the first line to multiple lines and make them in the order of time:

	```
	// Copyright 2011 Gary Burd
	// Copyright 2013 Unknwon
	```
	
- Spefify which license is used for the project in the README file:

	```
	## License
	
	This project is under the MIT License. See the [LICENSE](LICENSE) file for the full license text.
	```
# Project Structure

The following content shows a general structure of a web app, it can be changed based on the different conventions of web frameworks.

```
- templates (views)          # Template files
- public (static)            # Public assets
	- css
	- fonts
	- img
	- js
- routers(controllers)
- models
- modules
	- setting                 # Global settings of app
- cmd                         # Files of commands
- conf                        # Default configuration
	- locale                  # I18n locale files
- custom                      # Custom configuraion
- data                        # App generated data
- log                         # App generated log
```

## Command Line App

When writing a CLI app, each command should be in a separate file under `/cmd` directory:

```
/cmd
	dump.go
	fix.go
	serve.go
	update.go
	web.go
```

# Import Packages

All package import paths must be a URL from a code hosting site except packages from standard library.

Therefore, there are four types of packages a source file could import:

1. Packages from standard library
2. Third-party packages
3. Packages in same organization but not in same repository
4. Packages in same repository

Basic rules:

- Use different sections to separate import paths by an empty line for two or more types of packages.
- Must not use `.` to simplify import path except in test files (`*_test.go`).
- Must not use relative path (`./subpackage`) as import path, all import paths must be `go get`-able.

Here is a complete example:

```go
import (
    "fmt"
    "html/template"
    "net/http"
    "os"

    "github.com/codegangsta/cli"
    "gopkg.in/macaron.v1"

    "github.com/gogits/git"
    "github.com/gogits/gfm"

    "github.com/gogits/gogs/routers"
    "github.com/gogits/gogs/routers/repo"
    "github.com/gogits/gogs/routers/user"
)
```

# Commentary

- All exported objects must be well-commented; internal objects can be commented as needed.
- If the object is countable and does not know the number of it, use singular form and present tense; otherwise, use plural form.
- Comment of package, function, method and type must be a complete sentence.
- First letters of sentences should be upper case, but lower case for phrases.
- Maximum length of a comment line should be 80 characters.

### Package

- Only one file is needed for package level comment.
- For `main` packages, a line of introduction is enough, and should start with project name:

	```Go
	// Gogs (Go Git Service) is a painless self-hosted Git Service.
	package main
	```

- For sub-packages of a complicated project, no package level comment is required unless it's a functional module.
- For simple non-`main` packages, a line of introduction is enough as well.
- For more complicated functional non-`main` packages, commonly should have some basic usage examples and must start with `Package <name>`:

	```Go
	/*
	Package regexp implements a simple library for regular expressions.

	The syntax of the regular expressions accepted is:

	    regexp:
	        concatenation { '|' concatenation }
	    concatenation:
	        { closure }
	    closure:
	        term [ '*' | '+' | '?' ]
	    term:
	        '^'
	        '$'
	        '.'
	        character
	        '[' [ '^' ] character-ranges ']'
	        '(' regexp ')'
	*/
	package regexp
	```

- For very complicated functional packages, you should create a [`doc.go`](https://github.com/robfig/cron/blob/master/doc.go) file for it.

### Type, Interface and Other Types

- Type is often described in singular form:

	```Go
	// Request represents a request to run a command.
	type Request struct { ...
	```

- Interface should be described as follows：

	```Go
	// FileInfo is the interface that describes a file and is returned by Stat and Lstat.
	type FileInfo interface { ...
	```


### Functions and Methods

- Comments of functions and methods must start with its name:

	```Go
	// Post returns *BeegoHttpRequest with POST method.
	```

- If one sentence is not enough, go to next line:

	```Go
	// Copy copies file from source to target path.
	// It returns false and error when error occurs in underlying function calls.
	```

- If the main purpose of a function or method is returning a `bool` value, its comment should start with `<name> returns true if`:

	```Go
	// HasPrefix returns true if name has any string in given slice as prefix.
	func HasPrefix(name string, prefixes []string) bool { ...
	```

### Other Notes

- When something is waiting to be done, use comment starts with `TODO:` to remind maintainers.
- When a known problem/issue/bug needs to be fixed/improved, use comment starts with `FIXME:` to remind maintainers.
- When something is too magic and needs to be explained, use comment starts with `NOTE:`:

	```Go
	// NOTE: os.Chmod and os.Chtimes don't recognize symbolic link,
	// which will lead "no such file or directory" error.
	return os.Symlink(target, dest)
	```
# Naming Rules

### File name

- The main entry point file of the application should be named as `main.go` or same as the application. For example, the entry point file of `Gogs` is named `gogs.go`.

### Functions and Methods

- If the main purpose of functions or methods is returning a `bool` type value, the name of function or method should starts with `Has`, `Is`, `Can` or `Allow`, etc.

	```go
	func HasPrefix(name string, prefixes []string) bool { ... }
	func IsEntry(name string, entries []string) bool { ... }
	func CanManage(name string) bool { ... }
	func AllowGitHook() bool { ... }
	```

### Constants

- Constant should use Camel Case style, and capitalize the first letter.

	```go
	const AppVer = "0.7.0.1110 Beta"
	```

- If you need enumerated type, you should define the corresponding type first:

	```go
	type Scheme string

	const (
		HTTP  Scheme = "http"
		HTTPS Scheme = "https"
	)
	```

- If functionality of the module is relatively complicated and easy to mixed up with constant name, you can add prefix to every constant:

	```go
	type PullRequestStatus int

	const (
		PULL_REQUEST_STATUS_CONFLICT PullRequestStatus = iota
		PULL_REQUEST_STATUS_CHECKING
		PULL_REQUEST_STATUS_MERGEABLE
	)
	```

### Variables

- A variable name should follow general English expression or shorthand.
- In relatively simple (less objects and more specific) context, variable name can use simplified form as follows:
    - `user` to `u`
    - `userID` to `uid`
- If variable type is `bool`, its name should start with `Has`, `Is`, `Can` or `Allow`, etc.

	```go
	var isExist bool
	var hasConflict bool
	var canManage bool
	var allowGitHook bool
	```

- The last rule also applies for defining structs:

	```go
	// Webhook represents a web hook object.
	type Webhook struct {
		ID           int64 `xorm:"pk autoincr"`
		RepoID       int64
		OrgID        int64
		URL          string `xorm:"url TEXT"`
		ContentType  HookContentType
		Secret       string `xorm:"TEXT"`
		Events       string `xorm:"TEXT"`
		*HookEvent   `xorm:"-"`
		IsSSL        bool `xorm:"is_ssl"`
		IsActive     bool
		HookTaskType HookTaskType
		Meta         string     `xorm:"TEXT"` // store hook-specific attributes
		LastStatus   HookStatus // Last delivery status
		Created      time.Time  `xorm:"CREATED"`
		Updated      time.Time  `xorm:"UPDATED"`
	}
	```

#### Variable Naming Convention

Variable name is generally using Camel Case style, but when you have unique nouns, should apply following rules:

- If the variable is private, and the unique noun is the first word, then use lower cases, e.g. `apiClient`.
- Otherwise, use the original cases for the unique noun, e.g. `APIClient`, `repoID`, `UserID`.

Here is a list of words which are commonly identified as unique nouns:

```go
// A GonicMapper that contains a list of common initialisms taken from golang/lint
var LintGonicMapper = GonicMapper{
	"API":   true,
	"ASCII": true,
	"CPU":   true,
	"CSS":   true,
	"DNS":   true,
	"EOF":   true,
	"GUID":  true,
	"HTML":  true,
	"HTTP":  true,
	"HTTPS": true,
	"ID":    true,
	"IP":    true,
	"JSON":  true,
	"LHS":   true,
	"QPS":   true,
	"RAM":   true,
	"RHS":   true,
	"RPC":   true,
	"SLA":   true,
	"SMTP":  true,
	"SSH":   true,
	"TLS":   true,
	"TTL":   true,
	"UI":    true,
	"UID":   true,
	"UUID":  true,
	"URI":   true,
	"URL":   true,
	"UTF8":  true,
	"VM":    true,
	"XML":   true,
	"XSRF":  true,
	"XSS":   true,
}
```

# Declaration

### Functions or Methods

Order of arguments of functions or methods should generally apply following rules (from left to right):

1. More important to less important.
2. Simple types to complicated types.
3. Same types should be put together whenever possible.

#### Example

In the following declaration, type `User` is more complicated than type `string`, but `Repository` belongs to `User`, so `User` is more left than `Repository`.

```Go
func IsRepositoryExist(user *User, repoName string) (bool, error) { ...
```

# Coding Guidelines

### Basic Rules

- All applications that have `main` package should have constant `AppVer` to indicate its version with format `X.Y.Z.Date [Status]`. For example, `0.7.6.1112 Beta`.
- A library/package should have function `Version` to return its version in `string` type with format `X.Y.Z[.Date]`.
- You should consider split lines of code that have more than 80 characters. The rule is use to argument as the unit to split:

	```go
	So(z.ExtractTo(
		path.Join(os.TempDir(), "testdata/test2"),
		"dir/", "dir/bar", "readonly"), ShouldBeNil)
	```

- When the function or method's declaration has more than 80 characters, you should consider split it as well. The rule is arguments with same type in the same line. After declaration, you also need to put an empty line to distinguish itself with function or method's body:

	```go
	// NewNode initializes and returns a new Node representation.
	func NewNode(
		importPath, downloadUrl string,
		tp RevisionType, val string,
		isGetDeps bool) *Node {

		n := &Node{
			Pkg: Pkg{
				ImportPath: importPath,
				RootPath:   GetRootPath(importPath),
				Type:       tp,
				Value:      val,
			},
			DownloadURL: downloadUrl,
			IsGetDeps:   isGetDeps,
		}
		n.InstallPath = path.Join(setting.InstallRepoPath, n.RootPath) + n.ValSuffix()
		return n
	}
	```

- Group declaration should be organized by types, not all together:

	```go
	const (
		// Default section name.
		DEFAULT_SECTION = "DEFAULT"
		// Maximum allowed depth when recursively substituing variable names.
		_DEPTH_VALUES = 200
	)

	type ParseError int

	const (
		ERR_SECTION_NOT_FOUND ParseError = iota + 1
		ERR_KEY_NOT_FOUND
		ERR_BLANK_SECTION_NAME
		ERR_COULD_NOT_PARSE
	)
	```

- When there are more than one functional modules in single source file, you should use [ASCII Generator](http://www.network-science.de/ascii/) to make a highlight title:

	```go
	// _________                                       __
	// \_   ___ \  ____   _____   _____   ____   _____/  |_
	// /    \  \/ /  _ \ /     \ /     \_/ __ \ /    \   __\
	// \     \___(  <_> )  Y Y  \  Y Y  \  ___/|   |  \  |
	//  \______  /\____/|__|_|  /__|_|  /\___  >___|  /__|
	//         \/             \/      \/     \/     \/
	```

- Functions or methods are ordered by the dependency relationship, such that the most dependent function or method should be at the top. In the following example, `ExecCmdDirBytes` is the most fundamental function, it's called by `ExecCmdDir`, and `ExecCmdDir` is also called by `ExecCmd`:

	```go
	// ExecCmdDirBytes executes system command in given directory
	// and return stdout, stderr in bytes type, along with possible error.
	func ExecCmdDirBytes(dir, cmdName string, args ...string) ([]byte, []byte, error) {
		...
	}

	// ExecCmdDir executes system command in given directory
	// and return stdout, stderr in string type, along with possible error.
	func ExecCmdDir(dir, cmdName string, args ...string) (string, string, error) {
		bufOut, bufErr, err := ExecCmdDirBytes(dir, cmdName, args...)
		return string(bufOut), string(bufErr), err
	}

	// ExecCmd executes system command
	// and return stdout, stderr in string type, along with possible error.
	func ExecCmd(cmdName string, args ...string) (string, string, error) {
		return ExecCmdDir("", cmdName, args...)
	}
	```

- Methods of struct should be put after struct definition, and order them by the order of fields they mostly operate on:

	```go
	type Webhook struct { ... }
	func (w *Webhook) GetEvent() { ... }
	func (w *Webhook) SaveEvent() error { ... }
	func (w *Webhook) HasPushEvent() bool { ... }
	```

- If a struct has operational functions, should basically follow the `CRUD` order:

	```go
	func CreateWebhook(w *Webhook) error { ... }
	func GetWebhookById(hookId int64) (*Webhook, error) { ... }
	func UpdateWebhook(w *Webhook) error { ... }
	func DeleteWebhook(hookId int64) error { ... }
	```

- If a struct has functions or methods start with `Has`, `Is`, `Can` or `Allow`, they should be ordered by the same order of `Has`, `Is`, `Can` or `Allow`.
- Declaration of variables should be put before corresponding functions or methods:

	```go
	var CmdDump = cli.Command{
		Name:  "dump",
		...
		Action: runDump,
		Flags:  []cli.Flag{},
	}

	func runDump(*cli.Context) { ...
	```

- Use named field to initialize struct fields whenever possible:

	```go
	AddHookTask(&HookTask{
		Type:        HTT_WEBHOOK,
		Url:         w.Url,
		Payload:     p,
		ContentType: w.ContentType,
		IsSsl:       w.IsSsl,
	})
	```

# Test Cases

- Unit tests must use [GoConvey](http://goconvey.co/) and code coverage must above 80%.

### Examples

- The file of examples of helper modules should be named `example_test.go`.
- Test cases of functions must start with `Test_`, e.g. `Test_Logger`.
- Test cases of methods must use format `Text_<Struct>_<Method>`, e.g. `Test_Macaron_Run`.
