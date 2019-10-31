

# AsciiEDoc - EDoc extension for generating HTML or Github-flavored Markdown from AsciiDoc sources #

This fork is based on `asciidoctor` with support for embedded diagrams.

## Prerequests ##

Install [Asciidoctor and AsciiDoctor Diagram](https://asciidoctor.org/docs/asciidoctor-diagram/)

## Configuration ##

Copy [doc/asciidoc_stylesheet.css](doc/asciidoc_stylesheet.css) to your `rebar` project.

Add something similar to your `rebar.config`:

```erlang
{profiles, [{docs, [{edoc_opts, [{doclet, asciiedoc_doclet}
                                , {new, true}
                                , {subpackages, false}
                                , {image, ""} % don't copy erlang.png
                                , {stylesheet_file, "doc/asciidoc_stylesheet.css"}
                                ]}
                   , {deps, [{asciiedoc, {git, "git://github.com/eryx67/asciiedoc.git", {branch, "master"}}}
                            ]}
                   ]}
           ]}
```

To copy your `overview.edoc` to `README.md` you can use `pandoc`, example for `Makefile`:

```
doc: edoc

readme: doc
	asciidoc -b docbook45 -o - doc/overview.edoc | \
	sed -E 's@([>"])(img/)@\1doc/\2@g' | \
	pandoc -s -f docbook -t gfm -o README.md

```
