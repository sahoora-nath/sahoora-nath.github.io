---
layout: post
title: "Spring Boot, Failure Analyzer"
date: 2018-05-02
---

To create our own custom FailureAnalyzer, we can use AbstractFailureAnalyzer as our convenient extension point. 'AbstractFailureAnalyzer' will check if a specified exception is present and will allow our custom analyzer to handle it.
