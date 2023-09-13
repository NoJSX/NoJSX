# NoJSX

NoJSX is a [Vanilla JS framework](http://vanilla-js.com/) plugin that adds support for DOM tree declaration.

## Installation

Add the following code snippet to the HTML of your application.

```html
<script>
    function e(tag, attrs, children) {
        var element = document.createElement(tag);
        for (var name in attrs) element.setAttribute(name, attrs[name]);
        for (var i in children) element.appendChild(children[i]);
        return element;
    }
    function t(text) {
        return document.createTextNode(text);
    }
</script>
```

## Example application

```html
<div id="app">
    This application (unfortunately) requires JavaScript.
</div>
<script>
    var title, content, article, app = document.getElementById("app");
    app.innerHTML = "";
    [
        e("p", {}, [
            e("label", {for: "title"}, [t("Title")]),
            e("br"),
            title = e("input", {id: "title", value: "Hello, world!"}),
        ]),
        e("p", {}, [
            e("label", {for: "content"}, [t("Content")]),
            e("br"),
            content = e("textarea", {
                id: "content",
                rows: "8",
                cols: "80"
            }, [
                t("This application converts plain text to HTML.")
            ])
        ]),
        article = e("article")
    ].forEach(function (c) { app.appendChild(c); });

    function update() {
        
        var paragraphs = content.value
            .replace("\r\n", "\n")
            .replace("\r", "\n")
            .replace(/\n\n+/g, "\n\n")
            .split("\n\n");
        
        article.innerHTML = "";

        article.appendChild(e("h1", {}, [t(title.value)]));

        for (var i in paragraphs) {
            
            var paragraph = paragraphs[i];
            var lines = paragraph.split("\n");
            var p = e("p", {}, [t(lines[0])]);
            
            article.appendChild(p);

            if (lines.length == 1) {
                continue;
            }

            for (var j = 1; j < lines.length; j++) {
                p.appendChild(e("br"));
                p.appendChild(t(lines[j]));
            }
        }
    }

    update();
    title.addEventListener("keyup", update);
    content.addEventListener("keyup", update);
</script>
```
