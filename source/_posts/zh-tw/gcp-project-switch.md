---
title: gcp-project-switch
date: 2022-08-20 18:52:22
tags:
categories:
---

在 GCP 上部屬不同專案時需要切換專案，根據專案 ID 來切換：

# 查看專案 ID
{% codeblock GCP %}
gcloud projects list

PROJECT_ID: XXX
NAME: baby-bill
PROJECT_NUMBER: XXXXX
{% endcodeblock %}

# 切換專案
{% codeblock GCP %}
gcloud config set project `PROJECT ID`
{% endcodeblock %}