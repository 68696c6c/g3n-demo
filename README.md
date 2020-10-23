# G3n Demo

To run the demo:

```bash
go mod vendor
go run main.go
```

You will see an error like this:
```bash
# github.com/go-gl/glfw/v3.2/glfw
vendor/github.com/go-gl/glfw/v3.2/glfw/c_glfw.go:4:10: fatal error: glfw/src/context.c: No such file or directory
    4 | #include "glfw/src/context.c"
      |          ^~~~~~~~~~~~~~~~~~~~
```

Notice that it is trying to import glfw/v3.2 but g3n is hardcoded to use 3.3 in `window/glfw.go` line 77 and 78.

The problem seems to happen because `window/glfw.go` and `window/window.go` both import "github.com/go-gl/glfw/v3.2/glfw", not "github.com/go-gl/glfw/v3.3/glfw".

If you edit those files in the `vendor` directory to import "github.com/go-gl/glfw/v3.3/glfw" instead the demo will run.

The "github.com/go-gl/glfw/v3.2/glfw" package does not include a dummy.go file so the C files it contains are stripped out of the final build.  Because of this, I don't think it is possible for G3n to use it, so the hardcoded 3.3 version makes sense.  I think the imports just need to be updated to match.
