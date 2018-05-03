# SVG fragment builds for [reveal.js](https://github.com/hakimel/reveal.js)

## Basic use case
- make an SVG (maybe in [inkscape](http://www.inkscape.org/en/))
  - save it someplace reveal.js can find it (maybe next to your presentation)
  - figure out how to identify them (maybe use named layers)
- in reveal.js/index.html
  - add `reveal-svg-fragment.js` as a dependency
  - in a `<section>` of reveal.js markup
    - add `data-svg-fragment="<url of the someplace>"` to something, e.g.
      a `div`
    - add some things with `class="fragment"` inside that thing
      - add `title="<a selector>"` to those things
        - `[*|label=<a label>]` is good
        - for more about selectors, check out the
          [W3C page](http://www.w3.org/TR/css3-selectors/)

## Example
Let's assume I made an SVG in Inkscape, and saved it next to my `index.html`.
It has three layers: `base`, `fragment1` and `fragment2`.

    <html>
      ...
      <section>
        <div data-svg-fragment="figure.svg#[*|label=base]">
          <a class="fragment" href="[*|label=fragment1]"></a>
          <a class="fragment" href="[*|label=fragment2]"></a>
        </div>
      </section>
      ...
      <script>
        ...
        Reveal.initialize({
          dependencies: [
            ...
            {
              // maybe you put this in `plugins`
              src: 'reveal-svg-fragment.js',
              condition: function(){
                return !!document.querySelector( '[data-svg-fragment]' );
              }
              // Additional options
              // defaults to using already-loaded version, or CDN
              //d3: "./d3.min.js",
              // use a different attribute for your fragment selector
              //selector: "title",
            }
            ...
          ]
        ...
        }
        ...
      </script>
      ...
    </html>

## LIMITATIONS
- won't work in Chrome against `file://`
  - workarounds:
    - dropbox
    - sharepoint
    - confluence
    - gist
    - `python -m SimpleHTTPServer`
- probably won't work in IE
  - wontfix