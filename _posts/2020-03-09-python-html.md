---
layout: post
title:  "Building an HTML Library with Python"
date:   2020-03-09 12:00:00 -0700
image: '/assets/divs.jpg'
imageAttributeUrl: 'https://unsplash.com/@flac666'
imageAttributeName: 'Dominik Malinowski'
categories: projects
---

In this tutorial I will be walking through the development of my HTML builder Python package.

The completed package is available at [bvennes/html_builder](https://github.com/b-vennes/html_builder) on GitHub.

## Creating a Clientside Project With Python

The purpose of the HTML builder is to programmatically create an HTML file using Python. In the future, however, I would like to make SCSS and JavaScript builders as well so that the entirety of a web application could be built using Python.

HTML is a good place to start because the structure of an HTML file is fairly simple. HTML tags look like `<tag-name class="class-1 class-2"></tag-name>` and are nested to build out component hierarchies.

An HTML tag can be broken down into 4 major components:

- name of the tag
- list of classes for the tag
- additional attributes like `onclick` or `style`
- child tags or text

## Html Python Class

To create an HTML tag in Python, I created a basic object called `Html` holding access to the HTML tag's name, class names, and attributes like `onclick: doSomething()`.

It would also be possible for the classes list to be given as an attribute, since `class` is technically just an attribute, but this is a good opportunity to make use of Python's `*args` functionality.

Here is what the initial code looked like for the Html object:

``` python
class Html:
    def __init__(self, tag_name, *class_names, **attributes):
        """Initializes a new html tag."""
        self.name = tag_name
        self.class_names = list(class_names)
        self.attributes = attributes
        self.children = []
```

## Render Method

In order to output the Html object as a string, I added a `render` method.

``` python
def render(self):
        """Renders the html tag as a string."""
        html = f'<{self.name}'

        if self.class_names.__len__() > 0:
            classes_list = ' '.join(self.class_names)
            html += f' class="{classes_list}"'

        for key, value in self.attributes.items():
            html += f' {key}="{value}"'

        html += f'></{self.name}>'

        return html
```

To render the HTML tag as a string, we maintain an html element that begins with `<self.name`, add the classes as a list separated by a space, add the sets of attribute key/value pairs, and finally close it off with `></self.name>`. 

With both the `__init__` and `render` methods completed, the `Html` class looks like:

``` python
class Html:
    def __init__(self, tag_name, *class_names, **attributes):
        """Initializes a new html tag."""
        self.name = tag_name
        self.class_names = list(class_names)
        self.attributes = attributes
        self.children = []

    def render(self):
        """Renders the html tag as a string."""
        html = f'<{self.name}'

        if self.class_names.__len__() > 0:
            classes_list = ' '.join(self.class_names)
            html += f' class="{classes_list}"'

        for key, value in self.attributes.items():
            html += f' {key}="{value}"'

        html += f'></{self.name}>'

        return html
```

## Html Class Testing

Before testing this class out, let's setup the full `html_builder` Python package.

First, create a new directory called _html_builder_.

Add a blank file within this directory called \_\_init\_\_.py

Add a subdirectory to _html_builder_ also called _html_builder_.

Within the _html_builder_ subdirectory, add a file named _html.py_ and copy the `Html` class from above.

Then, create a test script in the same folder as the top-level _html_builder_ directory named _html_test.py_.

``` python
# html_test.py
from html_builder.html import Html

button = Html('button', 'class-1', 'class-2', onclick="alert('Hello world!')")

print(button.render())

```

The output of this Python script should be `<button class="class-1 class-2" onclick="alert('Hello world!')"></button>`. Let's copy this to a file named _button.html_ and open it in a web browser. We should see a tiny button with no text. If we click on it, the window alerts us with the message `Hello world!`. That's a promising start!

But users will want to be able to add text to their button. In order to do this, we want our button to be able to contain some child elements, like this:

``` html
<button class="class-1 class-2" onclick="alert('Hello world!')">
    Click me!
</button>
```

## Html Children

Let's add a child element to our button.

``` python
button = Html('button', onclick="alert('Hello world!')")
button.children += ['Click me!']
```

However, our render method isn't rendering any of our children, so let's fix that by adding a method called `render_children()`. We will make it a private method so that we can encapsulate any additional logic that might occur while rendering the tag's children. To make the method private, add `__` before the method name.

This is also a good time to make the HTML format nicely when printed using newline characters and spaces. 

``` python
def __render_children(self):
    """Renders the tag's children"""
    rendered_children = ''

    for child in self.children:
        rendered_children += '\n    '
        if type(child) is Html:
            rendered_children += child.render().replace('\n','\n    ')
        else:
            rendered_children += child
    
    rendered_children += '\n'
    
    return rendered_children
```

Let's also modify the `render` to use the new private method.

``` python
def render(self):
    """Renders the html tag as a string."""
    html = f'<{self.name}'

    if self.class_names.__len__() > 0:
        classes_list = ' '.join(self.class_names)
        html += f' class="{classes_list}"'
        
    for key, value in self.attributes.items():
        html += f' {key}="{value}"'

    rendered_children = self.__render_children()

    html += f'>{rendered_children}</{self.name}>'

    return html
```

The `Html` class should now look like this:

``` python
# html.py
class Html:
    def __init__(self, tag_name, *class_names, **attributes):
        """Initializes a new html tag."""
        self.name = tag_name
        self.class_names = list(class_names)
        self.attributes = attributes
        self.children = []

    def render(self):
        """Renders the html tag as a string."""
        html = f'<{self.name}'

        if self.class_names.__len__() > 0:
            classes_list = ' '.join(self.class_names)
            html += f' class="{classes_list}"'
            
        for key, value in self.attributes.items():
            html += f' {key}="{value}"'

        rendered_children = self.__render_children()

        html += f'>{rendered_children}</{self.name}>'

        return html

    def __render_children(self):
        """Renders the tag's children"""
        rendered_children = ''

        for child in self.children:
            rendered_children += '\n    '
            if type(child) is Html:
                rendered_children += child.render().replace('\n','\n    ')
            else:
                rendered_children += child
        
        rendered_children += '\n'
        
        return rendered_children
```

Looks good! Now we'll want to update our test script to make use of the new functionality. I've added `div` and `title` elements to test out nested HTML.

## Testing Child HTML Tags

``` python
# html_test.py
from html_builder.html_builder.html import Html

div = Html('div')

title = Html('h1')

button = Html('button', onclick="alert('Hello world!')")

button.children += ['Click me!']
title.children += ['HTML Builder Test']
div.children += [title, button]

print(div.render())
```

After running the _html_test.py_ script we should see the output

``` html
<div>
    <h1>
        HTML Builder Test
    </h1>
    <button onclick="alert('Hello world!')">
        Click me!
    </button>
</div>
```

After copying the output to _test.html_ and reloading the browser, it should show our title and a button that says 'Click me!'.

## Rendering to an HTML File

At this point, our builder is just about done. But it might be helpful for our users to render HTML directly into a file. So I've added an `output_file` parameter to the `render` method so that users can specify a path where the rendered HTML should go. Before returning the HTML as a string, I've also added a section for opening the file if it is given, writing to it, and closing it.

``` python
def render(self, output_file_path=None):
    """Renders the html tag as a string."""
    html = f'<{self.name}'

    if self.class_names.__len__() > 0:
        classes_list = ' '.join(self.class_names)
        html += f' class="{classes_list}"'
        
    for key, value in self.attributes.items():
        html += f' {key}="{value}"'

    rendered_children = self.__render_children()

    html += f'>{rendered_children}</{self.name}>'

    if output_file_path != None:
        output_file = open(output_file_path, 'w')
        output_file.write(html)
        output_file.close()

    return html
```

To verify this behavior works correctly, I've specified the output file in the test script.

``` python
from html_builder.html_builder.html import Html

div = Html('div')

title = Html('h1')

button = Html('button', onclick="alert('Hello world!')")

button.children += ['Click me!']
title.children += ['HTML Builder Test']
div.children += [title, button]

print(div.render('./test.html'))
```

Now when we run the script, the HTML is printed to both the output terminal and the _test.html_ file. Neat!
