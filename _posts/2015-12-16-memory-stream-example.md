---
layout: post
title:  "Reading and writing from a .NET MemoryStream"
date:   2015-12-16 09:00:00
categories: dotnet streams 
---

This is a quick little gist that illustrates how to read and write to a .NET
stream. It's a simple thing, very useful when you consider that a HttpWebRequest and HttpWebResponse 
objects use GetRequestStream() and GetResponseStream()

<script src="https://gist.github.com/lukemuccillo/06222e67dc96638392a4.js"></script>