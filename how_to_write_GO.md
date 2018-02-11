# How to write Go code

## Main practices

1. All go code on a single workspace
2. Workspace contain many version control repositories (git for example)
3. Each repository contains one or more packages
4. Each package consists one or more Go source files in a single directory.
5. The path to a package's directory determines its import path

### TL;DR

#### GOPATH

```
export GOPATH=/preferred/directory
export PATH=$PATH:$(go env GOPATH)/bin
```

#### Import paths

```
mkdir -p $GOPATH/src/github.com/user
```

#### Build and install program

```
go install github.com/user/hello
```

#### Build library

```
go build github.com/user/stringutil
```

#### Testing

Create a test file with _test.go and function named TestXXX with signature `func (t *testing.T)`.

```
go test github.com/user/stringutil
```

#### Remote packages

```
go get github.com/golang/example/hello
```

#### Cross compilng

```
GOOS=darwin GOARCH=amd386 go install github.com/user/hello
```

### Workspace example:

```
bin/
    hello                          # command executable
    outyet                         # command executable
pkg/
    linux_amd64/
        github.com/golang/example/
            stringutil.a           # package object
src/
    github.com/golang/example/
        .git/                      # Git repository metadata
	hello/
	    hello.go               # command source
	outyet/
	    main.go                # command source
	    main_test.go           # test source
	stringutil/
	    reverse.go             # package source
	    reverse_test.go        # test source
    golang.org/x/image/
        .git/                      # Git repository metadata
	bmp/
	    reader.go              # package source
	    writer.go              # package source
    ... (many more repositories and packages omitted) ...
```

### The GOPATH

The GOPATH define the default diretory to your Workspace. It must be different than Go installation path.

Verfify your GOPATH:

```
go env GOPATH
```

If the GOPATH is not set the default location wil be used, tipically $HOME/go ou %USERPROFILE%\go. If you want to set you GOPATH path to another place export the GOPATH with your preferred directory:

```
export GOPATH=/preferred/directory
```

For convenience export the bin path to:

```
export PATH=$PATH:$(go env GOPATH)/bin
```

If you need help with your GOPATH run `go help gopath`.

### Import paths

The import path uniquelly identifies a package. The package's import path corresponds to its location inside the workspace or i a remote repository.

The standard library packages haves short import paths like `fmt` or `net/http`.

For your own packages you have to choose a base path that does not colides with the standard packages or any other packages of Golang World.

The convention is to use something like `github.com/user` to avoid colisions. This not meas that you have to publish your code until it is ready to be published, but you have to dela with that thinking as you will publish in the future.

Example of some base path to your Go source codes:

```
mkdir -p $GOPATH/src/github.com/user
```

### First and incredible program (yes a hello world example)

Create a new directory on your workspace to put the source code:

```
mkdir $GOPATH/src/github.com/user/hello
```

Create a file called `hello.go` and put the code bellow inside:

```
package main

import "fmt"

func main() {
	fmt.Printf("Hello, world.\n")
}
```

Now build and install the program using the `go` tool:

```
go install github.com/user/hello
```

You can run this command anywhere on your system, `go` will find the package source to compile using the GOPATH.

You can also omit the package name if you are ont the package directory:

```
cd $GOPATH/src/github.com/user/hello
go install
```

If you are using a source control system you can initialize your repository using the commands bellow:

```
cd $GOPATH/src/github.com/user/hello
git init
  Initialized empty Git repository in /home/user/work/src/github.com/user/hello/.git/
git add hello.go
git commit -m "initial commit"
 [master (root-commit) 0b4507d] initial commit
 1 file changed, 1 insertion(+)
  create mode 100644 hello.go
```

### Your first library

Create a new directory to store the source code of the library:

```
mkdir $GOPATH/src/github.com/user/stringutil
```

Create a new file called `reverse.go` ont the new directory and put the code bellow inside:

```
// Package stringutil contains utility functions for working with strings.
package stringutil

// Reverse returns its argument string reversed rune-wise left to right.
func Reverse(s string) string {
	r := []rune(s)
	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
		r[i], r[j] = r[j], r[i]
	}
	return string(r)
}
```

Build the library:

```
go build github.com/user/stringutil
```

or if you are in the library directory

```
go build
```

Now change the `hello.go` to use the new library, edit the source code and insert the code as follows:

```
package main

import (
	"fmt"

	"github.com/user/stringutil"
)

func main() {
	fmt.Printf(stringutil.Reverse("!oG ,olleH"))
}
```

Build and install the new version

```
go install github.com/user/hello
```

Run the new version of `hello` to verify the result.

### Workspace after hello program

`tree $GOPATH`

```
.
├── bin
│   ├── gopkgs
│   └── hello
├── pkg
│   └── linux_amd64
│       └── github.com
│           ├── user
│           │   └── stringutil.a
│           ├── MichaelTJones
│           │   └── walk.a
│           └── uudashr
│               └── gopkgs.a
└── src
    └── github.com
        ├── user
        │   ├── hello
        │   │   └── hello.go
        │   └── stringutil
        │       └── reverse.go
        ├── MichaelTJones
        │   └── walk
        │       ├── path_plan9.go
        │       ├── path_unix.go
        │       ├── path_windows.go
        │       ├── README.md
        │       ├── symlink.go
        │       ├── symlink_windows.go
        │       ├── walk.go
        │       └── walk_test.go
        └── uudashr
            └── gopkgs
                ├── cmd
                │   └── gopkgs
                │       └── main.go
                ├── doc.go
                ├── Gopkg.lock
                ├── gopkgs.go
                ├── gopkgs_test.go
                ├── Gopkg.toml
                ├── LICENSE
                ├── Makefile
                └── README.md
```

The Go executables are statically linked ant you need nothing more than the executable to run the program on the destionation target. You just need to compile one version for each target (operating systems).

### Package names

The first statement on Go files must be the package name:

```
package name
```

Name will be the default packge name for imports.

The Go conventions that the package name is the last element of the import path, the package imported as `somelib/mystragename` should be named as `mystragename`.

Executable commands must always use `package main`.

### Testing

Go test framework knows test files with suffix _test.go that contains functions named TestXXX with signature `func (t *testing.T)`.

Create a test to test stringutil package. Create a file called revers_test.go on $GOPATH/src/github.com/user/stringutil with the following content:

```
package stringutil

import "testing"

func TestReverse(t *testing.T) {
	cases := []struct {
		in, want string
	}{
		{"Hello, world", "dlrow ,olleH"},
		{"Hello, 世界", "界世 ,olleH"},
		{"", ""},
	}
	for _, c := range cases {
		got := Reverse(c.in)
		if got != c.want {
			t.Errorf("Reverse(%q) == %q, want %q", c.in, got, c.want)
		}
	}
}
```

Run the test:

```
go test github.com/user/stringutil
```

### Remote packages

You can get the remote packages using `go tool` to get packages from a Git or a Mercurial reposirtories.

```
go get github.com/golang/example/hello
```

### Cross compiling

Go support many operating systems and archs:

GOOS - Target Operating System | GOARCH - Target Platform
--- | ---
android | arm
darwin | 386
darwin | amd64
darwin | arm
darwin | arm64
dragonfly | amd64
freebsd | 386
freebsd | amd64
freebsd | arm
linux | 386
linux | amd64
linux | arm
linux | arm64
linux | ppc64
linux | ppc64le
linux | mips
linux | mipsle
linux | mips64
linux | mips64le
netbsd | 386
netbsd | amd64
netbsd | arm
openbsd | 386
openbsd | amd64
openbsd | arm
plan9 | 386
plan9 | amd64
solaris | amd64
windows | 386
windows | amd64

To compile and generate the executable for other paltform runs the command below:

```
GOOS=darwin GOARCH=amd386 go install github.com/user/hello
```