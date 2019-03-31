---
title: A Quine in Odin
slug: quine-in-odin
author: Ginger Bill
date: '2019-03-10'
categories:
  - odin
tags:
  - odin quine
---

A [Quine](https://wikipedia.org/wiki/Quine_(computing)) in [Odin](/odin):

```odin
package quine

import "core:fmt"

main :: proc() {
	fmt.printf("%s%c%s%c;\n", s, 0x60, s, 0x60);
}

s := `package quine

import "core:fmt"

main :: proc() {
	fmt.printf("%s%c%s%c;\n", s, 0x60, s, 0x60);
}

s := `;
```
