[![Build Status](https://travis-ci.org/KeenS/zenekindarl.svg?branch=master)](https://travis-ci.org/KeenS/zenekindarl)
[![Coverage Status](https://coveralls.io/repos/KeenS/zenekindarl/badge.svg?branch=master)](https://coveralls.io/r/KeenS/zenekindarl)

# Zenekindarl
Expected to be a fast, flexible, extensible, low memory usage, async, concurrent template engine.

## Usage
Like this

```lisp
(render "Hello {{var name}}!!"
        :name "κeen")
```

```lisp
(let ((renderer (compile-template-string :stream "Hello {{var name}}!!")))
  (funcall renderer *standard-output* :name "κeen"))
```

.

For more information, see docstring 

## Instant Benchmark
Zenekindarl perform **x16** as fast as a template engine in Python in the following instant benchmark.

![Benchmark](https://docs.google.com/spreadsheets/d/1M8x9dcK8ToL4-tfVUfGnCh_OOtttJpXxK905raA0eas/pubchart?oid=1882415724&format=image)

Template engines     | Time[sec]
---------------------|----------
Zenekindarl, SBCL 1.1.8   | 1.365
Jinja2, Python 2.7.5 | 24.07

The benchmark code for Zenekindarl:

    > (time
       (with-open-file (out #P"~/Desktop/out" :direction :output :if-exists :supersede)
         (let ((fun (zenekindarl:compile-template-string :stream "Hello {{var name}}!!")))
           (loop repeat 1000000
              do (funcall fun out :name "κeen")))))
    Evaluation took:
    1.625 seconds of real time
    1.364707 seconds of total run time (1.302198 user, 0.062509 system)
    [ Run times consist of 0.042 seconds GC time, and 1.323 seconds non-GC time. ]
    84.00% CPU
    1 form interpreted
    3 lambdas converted
    3,265,218,807 processor cycles
    528,706,464 bytes consed

The benchmark code for a template engine in Python:

    $ cat te.py
    from jinja2 import Template
    
    template = Template( u'Hello {{ name }}!!' )
    
    f = open( 'out', 'w' )
    for i in range( 1000000 ):
      f.write( template.render( name=u'κeen' ).encode( 'utf-8' ) )

    $ time python te.py
    real    0m25.612s
    user    0m24.069s
    sys	    0m0.190s

## Syntax

### Variable
variables will be HTML escaped


```html
{{var foo}}
```

### Repeat

```html
{{repeat 10}}hello{{endrepeat}}
```

```
{{repeat n as i}}<li>{{var i}}th item</li>{{endrepeat}}
```

### Loop

```html
<ol>
  {{loop items as item}}
  <li>{{var item}}</li>
  {{endloop}}
</ol>
```

### If

```
{{if new-p}}New{{else}}Old{{endif}}
```


### Insert

```html
See code below
<code><pre>
{{insert "snippet.lisp"}}
</pre></code>
```

### Include


```html
<nav>
{{incude "sidebar.tmpl"}}
</nav>
```


## Author

* κeen

## Copyright

Copyright (c) 2014 κeen
