+++
title = "Add Line Breaks"
date = 2017-10-19T23:50:23-05:00
draft = true

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["notepad++"]
categories = ["code"]

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = ""
caption = ""
+++


# How to break lines at a specific character in Notepad++

Sometimes when editing files, you may encounter a nice long minified file that contains no line breaks and is really hard to read. This causes really long horizontal scroll bars on some editors, and in others a really odd looking text wrapping. By adding line breaks after a common character such as a comma, period bracket, brace or something else, these files can become less of a chore to edit.

Check out the below example of a time where I needed to do this.

The following text file contains data that I need to work with:


```
['22APR2012 23:10', '23APR2012 07:10', 1, 3, 0], ['22APR2012 23:10', '23APR2012 07:20', 1, 3, 0], ['22APR2012 23:15', '23APR2012 06:40', 0, 1, 0], ['22APR2012
23:15', '23APR2012 06:40', 1, 3, 0], ['22APR2012 23:15', '23APR2012 06:40', 0, 1, 0], ['22APR2012 23:15', '23APR2012 07:00', 1, 3, 0], ['22APR2012 23:15', '23APR2012
07:00', 0, 1, 0], ['22APR2012 23:20', '23APR2012 09:35', 0, 1, 0], ['22APR2012 23:20', '23APR2012 09:35', 1, 3, 0], ['22APR2012 23:20', '23APR2012 10:10', 1, 3, 0],
['22APR2012 23:25', '23APR2012 05:35', 1, 3, 0],
```
<center>
###### Note the horizontal scroll bar...
</center>

This is what I want it to look like:

```
['22APR2012 19:30', '23APR2012 00:25', 0, 1, 0],
['22APR2012 19:35', '23APR2012 01:45', 1, 3, 0],
['22APR2012 19:50', '23APR2012 05:25', 1, 3, 0],
['22APR2012 19:50', '23APR2012 05:25', 0, 1, 0],
['22APR2012 19:55', '23APR2012 06:25', 1, 3, 0],
```

To do this, lets add lines breaks after the `],` characters:

- Type `Ctrl + h` or "Search" -> "Replace" from the top menu
- Under the Search Mode group, select "Regular expression"
- In the "Find what text" field, type `],\s*`
- In the "Replace with text" field, type `],\n`
- Click "Replace All"

Now the text file is easy to read and edit.

Happy Coding!
