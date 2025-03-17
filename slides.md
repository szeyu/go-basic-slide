---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# some information about your slides (markdown enabled)
title: Golang Beginner Course
info: |
  ## Golang Beginner Course
  Essentials for getting started with Go programming
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
highlighter: shiki
lineNumbers: false
---

# Golang Beginner Course
## Essentials Only

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/golang/go" target="_blank" alt="GitHub" title="Go on GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
layout: default
---

# Course Overview

<Toc maxDepth="1"></Toc>

---
src: ./pages/introduction.md
---

---
src: ./pages/basics.md
---

---
src: ./pages/functions.md
---

---
src: ./pages/collections.md
---

---
src: ./pages/structs-pointers.md
---

---
src: ./pages/concurrency.md
---

---
src: ./pages/web-development.md
---

---
layout: center
class: text-center
---

# Thank You!

[Go Documentation](https://golang.org/doc/) · [Go by Example](https://gobyexample.com/)

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---
