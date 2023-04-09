# Heading #

## Second level ##

### Third level ###

# Text and paragraph #

Describe how physical quantity changes over time/space.

Ex: Sound, radio, image, movie,...

# Block quote #

This is a paragraph before the block quote.

> This is a block quote. This
> paragraph has two lines.
>
> 1. This is a list inside a block quote.
> 2. Second item.


# Code block #

The following is a code block:

~~~~ {#mycode .c .numberLines startFrom="1"}
int main(int argc, char *argv[])
{
    printf("Hello world!");
    return EXIT_SUCCESS;
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Line block #

Useful for verse and addresses (though I doubt it)

| The limerick packs laughs anatomical
| In space that is quite economical.
|    But the good ones I've seen
|    So seldom are clean
| And the clean ones so seldom are comical

| 200 Main St.
| Berkeley, CA 94718


# List #

## Unordered ##

This is a unordered list:

- one
- two
- three

You can put block content inside list:

* First paragraph.

  Continued.

* Second paragraph. With a code block, which must be indented to align with the second paragraph.

  ~~~
  This is some testing code block
  1 + 1 = 2
  print("Testint")
  ~~~

This is a nested list:

* fruits
  + apples
    - macintosh
    - red delicious
  + pears
  + peaches
* vegetables
  + broccoli
  + chard

## Ordered ##

Numbered by uppercase, lowercase letters, roman numerals, Arabic numerals

1.  one
2.  two
3.  three

---

a)  Mark
b)  by
c)  letters

---

(i)  Marked
(ii) by
(iii) roman numerals

Fancy list allow '#' to be used as ordered list marker:

#. one
#. two

We can adjust starting number of the list:

 9)  Ninth
10)  Tenth
11)  Eleventh
       i. subone
      ii. subtwo
     iii. subthree

New list created for new list marker. List can also be ended with HTML comment <!-- -->:

(2) Two
(5) Three
1.  Four
*   Five

Also task list (Github-Flavored Markdown):

- [ ] an unchecked task list item
- [x] checked item


# Definition list #

Term 1
:   Definition 1

Term 2 with *inline markup*
:   Definition 2

    ~~~
    some code, part of Definition 2
    ~~~

    Third paragraph of definition 2.


# Table #

## Simple table ##

A simple table with caption:

  Right     Left     Center     Default
-------     ------ ----------   -------
     12     12        12            12
    123     123       123          123
      1     1          1             1

Table:  Demonstration of simple table syntax.

## Multiline table ##

A multiline table with caption:

-------------------------------------------------------------
 Centered   Default           Right Left
  Header    Aligned         Aligned Aligned
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
-------------------------------------------------------------

: Here's the caption. It, too, may span
multiple lines.

## Grid table ##

: Sample grid table.

+---------------+---------------+--------------------+
| Fruit         | Price         | Advantages         |
+===============+===============+====================+
| Bananas       | $1.34         | - built-in wrapper |
|               |               | - bright color     |
+---------------+---------------+--------------------+
| Oranges       | $2.10         | - cures scurvy     |
|               |               | - tasty            |
+---------------+---------------+--------------------+

# Inline formating #

## Emphasis ##

This text is _emphasized with underscores_, and this is *emphasized with asterisks*.

This is **strong emphasis** and __with underscores__.

## Strikeout ##

This ~~is deleted text.~~

## Superscripts and subscripts ##

H~2~O is a liquid. 2^10^ is 1024.

P~a\ cat~, not P~a cat~.

## Verbatim ##

What is the difference between `>>=` and `>>`?

This is a backslash followed by an asterisk: `\*`.

Attributes can be attached to verbatim text, just as with fenced code blocks:

`int main();`{.c}

# Math #

The well known Pythagorean theorem $x^2 + y^2 = z^2$ was proved to be invalid for other exponents.
Meaning the next equation has no integer solutions:

$$
x^n + y^n = z^n
$$

Raw text can be inserted by using the `raw_attribute` extension:

```{=latex}
\begin{align*}
S(\omega)
&= \frac{\alpha g^2}{\omega^5} e^{[ -0.74\bigl\{\frac{\omega U_\omega 19.5}{g}\bigr\}^{\!-4}\,]} \\
&= \frac{\alpha g^2}{\omega^5} \exp\Bigl[ -0.74\Bigl\{\frac{\omega U_\omega 19.5}{g}\Bigr\}^{\!-4}\,\Bigr]
\end{align*}
```

# Link #

## Automatic links ##

<https://google.com>
<sam@green.eggs.ham>

## Inline link ##

This is an [inline link](/url), and here's [one with a title](https://fsf.org "click here for a good time!").

[Write me!](mailto:sam@green.eggs.ham)

## Reference links ##

These are several links:

- [Link 1][my label 1]
- [Link 2][my label 2]
- [Link 3][my label 3]
- [Link 4][my label 4]

[my label 1]: /foo/bar.html  "My title, optional"
[my label 2]: /foo
[my label 3]: https://fsf.org (The Free Software Foundation)
[my label 4]: /bar#special  'A title in single quotes'

## Internal link ## {#internal_link}

This is [a link](#internal_link) to this section.


# Images #

![This is the caption](image.jpg)

![movie reel]

[movie reel]: movie.gif

There are also image attributes. But skip for now.

# Footnotes #

Here is a footnote reference,[^1] and another.[^longnote]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

    Subsequent paragraphs are indented to show that they
belong to the previous footnote.

        { some.code }

    The whole paragraph can be indented, or just the first
    line.  In this way, multi-paragraph footnotes work like
    multi-paragraph list items.

This paragraph won't be part of the note, because it
isn't indented.

# Citation #

Skip for now
