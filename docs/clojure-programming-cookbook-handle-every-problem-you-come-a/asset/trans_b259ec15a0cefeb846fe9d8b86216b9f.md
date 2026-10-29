

"Divide by zero"

+   - Figwheel REPL

Infinity

The next code is another example of Reader Conditionals for try and catch. The check-range checks whether the given value v is between the lower l and higher h. If the value is out of range, this function throws an exception or error:

```
(defn check-range [v l u]
  (if-not (and (>= v l) (<= v u))
    #?(:clj (throw (Exception. "out of range"))
       :cljs (throw (js/Error. "out of range")))
    v))
```

We will use js/Error in ClojureScript instead of _Exception in Clojure:

```
(try
  (check-range 0 1 10)
  (catch
    #?(
      :clj Exception
      :cljs js/Error
      ) e
    #?(:clj (println (.getMessage e))
       :cljs (println (.-message e)))))
```

In ClojureScript, (.-message e) retrieves the error message. .-function calls host JavaScript functions.

## How it works...

Here, we will explain the form of Reader Conditionals and Splice macros.

## The form of Reader Conditionals and Splice macros

The form of Reader Conditionals is as follows:

```
#?(:clj  (Clojure expression)
:cljs (ClojureScript expression)
:clr (Clojure CLR expression))
```

When the preceding code runs on Clojure, (Clojure expression) is evaluated. When the code runs on ClojureScript, (ClojureScript expression) is evaluated and vice versa.

Splice Reader Conditional macros are the same as Reader Conditional macros, but an expression must be a vector. The expression will be spliced into the resulting read.

We will write the following code:

```
(defn test-splicing-hello []
  #?@(:clj [(print "hello ") (print "world !") (print "\n")])
;;=> #'cljc-example.demo/test-splicing-hello
```

The expression for the Splice Reader Conditional macro is a vector, and the result of executing the method is as follows:

```
(test-splicing-hello)
;;=> hello world !
;;=> nil
```

The preceding code is equivalent to the following one:

```
(defn test-hello []
  (print "hello ") (print "world !") (print "\n"))
;;=> #'cljc-example.demo/test-hello
```

Splice Reader Conditional macros cannot be located at top level. If we do that, an exception like the following is thrown:

```
#?@(:clj
  [(print "hello ") (print "world !") (print "\n")])
;;=> RuntimeException Reader conditional splicing not allowed at the top level. clojure.lang.Util.runtimeException (Util.java:221)
```

Splice Reader Conditional macros are often used in require/import. We will show an example in this recipe.

## Macros in ClojureScript

ClojureScript's macros are different from those of Clojure. In ClojureScript, macros are defined in a different compilation stage rather than the one from where they are consumed.

So, we need to use the `:include-macros` or `:require-macros` keyword in namespace declarations for ClojureScript.

The `:include-macros` is a sugar for `:require-macros`:

```
(:require [quil.core :as q :include-macros true])
```

The preceding code is the equivalent to the following one:

```
(:require [quil.core :as q])
(:require-macros [quil.core :as q])
```

## There's more...

We will use Quil, which is an interactive library for drawings and animations. Quil supports both Clojure and ClojureScript. We will learn how our single source code runs on two different environments.

## Building an application for Clojure and ClojureScript

We will learn a more complex usage of Reader Conditionals. We will develop a graphical demonstration for Clojure and ClojureScript.

So, we will create runtimes for Clojure and ClojureScript with a single source file (.cljc). We will use Quil, which is a Clojure/ClojureScript library for interactive graphics. It provides over 200 functions that allows it to draw in 2D, 3D, and even in PDF.

The original demo is in the home page in Quil, so let's see how to run it in both Clojure and ClojureScript.

Although the steps here are rather long, we will not only learn how to write code for Clojure and ClojureScript using Reader Conditionals, but

we will also learn steps for building and running the two different environments from single source code.

## Adding a cljc file for demonstrating Quil

Let's add the quil_demo.cljc file, which shows the Quil demo for Clojure and ClojureScript. The location of the file is under the src/cljc_example directory, as shown in the following screenshot:

![](img/d7b631352a782ee50160cf8eb995152f_466_0.png)

We will modify the generated project.clj. We will add the quil library as follows:

```
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.clojure/clojurescript "1.8.51"]
                [org.clojure/core.async "0.2.374"
                 :exclusions [org.clojure/tools.reader]]
                [sablono "0.3.6"]
                [org.omcljs/om "0.9.0"]
                [quil "2.4.0"]]
```

In the dependency section, we have added [quil "2.4.0"]. Then we will replace the main namespace from cljc-example.core to cljc-example.quil-demo in the cljsbuild section as follows:

```
:cljsbuild {:builds
```

```
[{:id "dev"
  :source-paths ["src"]
  ;; If no code is to be run, set :figwheel true
for continued automagical reloading
  :figwheel {:on-jsload "cljc-example.quil-demo/on-js-reload"}
  :compiler {:main cljc-example.quil-demo
             :asset-path "js/compiled/out"
             :output-to
"resources/public/js/compiled/cljc_example.js"
             :output-dir
"resources/public/js/compiled/out"
             :source-map-timestamp true}}
  ;; This next build is an compressed minified
build for
  ;; production. You can build this with:
  ;; lein cljsbuild once min
  {:id "min"
   :source-paths ["src"]
   :compiler {:output-to
"resources/public/js/compiled/cljc_example.js"
              :main cljc-example.quil-demo
              :optimizations :advanced
              :pretty-print false}}]
```

After we finish the preceding modifications, let's restart REPL and Figwheel REPL. We will declare quil and om libraries for Clojure and ClojureScript:

```
(ns cljc-example.quil-demo
  (:require
   #?@(
     :clj
     [[quil.core :as q]
      [quil.middleware :as m]]
     :cljs
     [[quil.core :as q :include-macros true]
      [quil.middleware :as m]
      [om.core :as om :include-macros true]
      [om.dom :as dom :include-macros true]]
     ))
  #?(:clj (:gen-class)))
```

The following code is for ClojureScript to add the canvas tag for drawing graphics used by Quil:

```
#?( :cljs (defn on-js-reload [] ;; optionally touch your app-state to force rerendering depending on ;; your application ;; (swap! app-state update-in [:__figwheel_counter] inc) )) #?( :cljs (def app-state (atom {:text "Hello quil demo !"}))) #?( :cljs (enable-console-print!)) #?( :clj (println "Using Clojure !") :cljs (println "Using Clojurescript !")) #?( :cljs (om/root (fn [data owner] (reify om/IRender (render [_] (dom/canvas #js {:id "my-sketch" :height 400 :width 400}))) ) app-state {:target (. js/document (getElementById "app"))}))
```

Code here is animating a circle for Clojure and ClojureScript. Fortunately, code is the same in them:

```
(defn setup [] (q/frame-rate 60) (q/color-mode :hsb) {:color 0 :angle 0}) (defn update-state [state] {:color (mod (+ (:color state) 0.7) 255) :angle (+ (:angle state) 0.1)}) (defn draw-state [state] (q/background 240) (q/fill (:color state) 255 255) (let [angle (:angle state) x (* 150 (q/cos angle))
```

```
y (* 150 (q/sin angle))]
(q/with-translation [(/ (q/width) 2)
                    (/ (q/height) 2)]
  (q/ellipse x y 50 50)))
```

```
(defn run[]
  (q/defsketch my-sketch
    :host "my-sketch"
    :size [500 500]
    :setup setup
    :update update-state
    :draw draw-state
    :middleware [m/fun-mode]))
```

setup is the initial setup function, and update - state updates the current state.

## Testing cljc code in both Clojure and ClojureScript

We will test the preceding code in REPLs in both Clojure and ClojureScript. Let's test it in the Clojure REPL first. Let's type the following:

```
(run)
```

Then, a new window will open and you can see the following:

![](img/d7b631352a782ee50160cf8eb995152f_469_0.png)

