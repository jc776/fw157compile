I can reproduce the bad path issue by doing a non-optimized compile, then an advanced compile:
```
PS C:\Users\Jack\SSD\Projects\test\fw157compile> clj -m figwheel.main -bo dev
2019-08-04 12:42:28.215:INFO::main: Logging initialized @7060ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Compiling build dev to "target\public\cljs-out\dev-main.js"
[Figwheel] Successfully compiled build dev to "target\public\cljs-out\dev-main.js" in 10.441 seconds.

PS C:\Users\Jack\SSD\Projects\test\fw157compile> clj -m figwheel.main --optimizations advanced -bo dev
2019-08-04 12:42:52.707:INFO::main: Logging initialized @6527ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Compiling build dev to "target\public\cljs-out\dev-main.js"
[Figwheel] Failed to compile build dev in 5.932 seconds.
[Figwheel:WARNING] Compile Exception: Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
[Figwheel:SEVERE] java.nio.file.InvalidPathException: Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
Execution error (InvalidPathException) at sun.nio.fs.WindowsPathParser/normalize (WindowsPathParser.java:182).
Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
```

Deleting 'target' before advanced compile works for me:
```
C:\Users\Jack\AppData\Local\Temp\clojure-17737711244158279339.edn
PS C:\Users\Jack\SSD\Projects\test\fw157compile> clj -m figwheel.main --optimizations advanced -bo dev
2019-08-04 12:44:07.989:INFO::main: Logging initialized @7043ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Compiling build dev to "target\public\cljs-out\dev-main.js"
[Figwheel] Successfully compiled build dev to "target\public\cljs-out\dev-main.js" in 20.38 seconds.
```

The first scenario still fails for me with updated cljs:
```
PS C:\Users\Jack\SSD\Projects\test\fw157compile> clj -A:cljs520 -m figwheel.main -bo dev
2019-08-04 12:45:19.692:INFO::main: Logging initialized @8981ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Compiling build dev to "target\public\cljs-out\dev-main.js"
[Figwheel] Successfully compiled build dev to "target\public\cljs-out\dev-main.js" in 9.215 seconds.

PS C:\Users\Jack\SSD\Projects\test\fw157compile> clj -A:cljs520 -m figwheel.main --optimizations advanced -bo dev
2019-08-04 12:45:51.621:INFO::main: Logging initialized @7018ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Compiling build dev to "target\public\cljs-out\dev-main.js"
[Figwheel] Failed to compile build dev in 6.14 seconds.
[Figwheel:WARNING] Compile Exception: Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
[Figwheel:SEVERE] java.nio.file.InvalidPathException: Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
Execution error (InvalidPathException) at sun.nio.fs.WindowsPathParser/normalize (WindowsPathParser.java:182).
Illegal char <:> at index 2: /C:/Users/Jack/SSD/Projects/test/fw157compile/target/public/cljs-out/dev/cljs/core.js
```

'target' isn't on the classpath in this case, but may also have an effect if using build-once from a REPL.