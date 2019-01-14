---
layout: post
title: "Test"
categories:
- tutorial
date: 2017-11-16 00:00:00
---
<p lang="zh">
推上濟唱線新車年苦，驚的但，示國不林人性早息、不紀管決，所現解往運聲最證一在著都職麼，年結書往王認？年資熱大外方全自中，帶表知聲用滿。精市時成隊，初果絕大南什文自數灣安獎晚學優現東情面然中展：經子還？要人家特個應學利。種行頭一，有公出小傳業叫。
</p>

<p lang="zh">
獨種輕臺電股發有生上你離裡清天書分，又顯所縣謝分亞上負生事的子第一同時？資不。
只的上此紙華單裡業明平是上輕們熱，還生如心外過構吃阿電家就回答系臺房出求位列長相熱知臺。
人候神傳的較在力續園色，天力天！他受上她車及？
</p>

<p lang="zh">
個行運支，紀主已學書打還此子著公大重真候把，這時老用入得半濟起大現入夜因曾上沒力路知變阿……說至產立：史晚象入。有坐認笑：失子重麼的動證。可臺是轉月記們很火：到看生工有具國陸不下將；花以臺親的？也著比中完者效對小中有驗了是那書，世賽野無或我發服別夠或球國快行什關系。曾先讀據議座，和性其觀易上去初會復生完正；校經在委為高寫定？論那求去得。
區大以像裡持也車太做日考地有雨裡動不雄港，別同不清少陽其的專界共布。
</p>

$$\sum_{i=1}^{N}A_i$$

This note demonstrates some of what [Markdown][1] is capable of doing.[^1]

*Note: Feel free to play with this page.*

```c
#ifndef TEST_H
#define TEST_H

#include <stdio.h>

/* comment */
int main(void)
{
	puts("Hello");
	return 0;
}

#endif
```

```python
import sys
import os

ENV = os.environ

@dec
def fun:
    return 1


class Foo(Bar):
    def bar(self, x, y=None):
        return x + y if y else x

if __name__ == "__main__":
    sys.exit(fun())
```

Java test
```java
class Foo {
    int a;
    long b;
    void Car(int a, long b) {
        this.a = a;
        this.b = b;
    }

    void foo(Bar b) {
        b.baz(this.b + 0.01 + 4);
    }
}
```
## Basic formatting

Paragraphs can be written like so. A paragraph is the basic block of Markdown. A paragraph is what text will turn into when there is no reason it should become anything else.

Paragraphs must be separated by a blank line. Basic formatting of *italics* and **bold** is supported. This *can be **nested** like* so.

## Lists

### Ordered list

1. Item 1
2. A second item
3. Number 3
4. Ⅳ

*Note: the fourth item uses the Unicode character for [Roman numeral four][2].*

### Unordered list

* An item[^2]
* Another item
* Yet another item
* And there's more...

## Paragraph modifiers

### Code block

    Code blocks are very useful for developers and other people who look at code or other things that are written in plain text. As you can see, it uses a fixed-width font.

You can also make `inline code` to add code into other things.

### Quote

> Here is a quote. What this is should be self explanatory. Quotes are automatically indented when they are used.

## Headings

There are six levels of headings. They correspond with the six levels of HTML headings. You've probably noticed them already in the page. Each level down uses one more hash character.

### Headings *can* also contain **formatting**

### They can even contain `inline code`

Of course, demonstrating what headings look like messes up the structure of the page.

I don't recommend using more than three or four levels of headings here, because, when you're smallest heading isn't too small, and you're largest heading isn't too big, and you want each size up to look noticeably larger and more important, there there are only so many sizes that you can use.

## URLs

URLs can be made in a handful of ways:

* A named link to [MarkItDown][3]. The easiest way to do these is to select what you want to make a link and hit `Ctrl+L`.
* Another named link to [MarkItDown](http://www.markitdown.net/)
* Sometimes you just want a URL like <http://www.markitdown.net/>.

## Horizontal rule

A horizontal rule is a line that goes across the middle of the page.

---

It's sometimes handy for breaking things up.

## Images

Markdown can also contain images. I'll need to add something here sometime.

## Emoji
:+1:

## Finally

There's actually a lot more to Markdown than this. See the official [introduction][4] and [syntax][5] for more information. However, be aware that this is not using the official implementation, and this might work subtly differently in some of the little things.


  [1]: http://daringfireball.net/projects/markdown/
  [2]: http://www.fileformat.info/info/unicode/char/2163/index.htm
  [3]: http://www.markitdown.net/
  [4]: http://daringfireball.net/projects/markdown/basics
  [5]: http://daringfireball.net/projects/markdown/syntax

[^1]: some footnote
[^2]: footnote test
