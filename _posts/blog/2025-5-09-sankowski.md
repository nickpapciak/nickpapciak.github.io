---
layout: post
title: Dynamic All-Pairs Shortest Paths
subtitle: Polynomial Matrix Inverses and Fast Edge Updates
categories: blog
permalink: blog/dynamic-apsp
mathjax: true
---

<!--more-->

<style>
.proof-box {
  background-color: #f9f9f9;
  border-left: 4px solid #ccc;
  padding: 1rem 1.25rem;
  margin: 2rem 0;
  border-radius: 6px;
  font-size: 1rem;
}

.proof-box::before {
  content: "🔍 Proof";
  display: block;
  font-weight: bold;
  margin-bottom: 0.5rem;
  color: #333;
}
</style>

## Introduction

This post will explore a dynamic APSP algorithm by Sankowski (2004)

(details found here https://faculty.cc.gatech.edu/~vdbrand/8803/fall22_daa_lecturenotes.pdf)
