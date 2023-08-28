---
title: Win11右键添加MD
tags:
  - windows
categories:
  - 杂项
abbrlink: 21ce9418
date: 2023-05-14 23:12
updated: 2023-05-14 23:12
---



```text
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\.md]
@="Typora.exe"
[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""
[HKEY_CLASSES_ROOT\Typora.exe]
@="Markdown"
```

*<!--more-->*

