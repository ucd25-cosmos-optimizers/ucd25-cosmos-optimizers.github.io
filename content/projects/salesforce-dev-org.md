+++
title = 'Salesforce Development Org'
date = 2024-06-03T10:53:14-04:00
weight = 20
description = "Salesforce Developer Environment"
tags = ['salesforce', 'salesforce org']
pageName = 'salesforce-dev-org'
icon = 'cloud'
draft = false
+++

<div style="display: flex; flex-direction: row;">
    <div><a href="https://github.com/k-capehart/sfdc-dev-org"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=whitef" alt="Link to Github repo"></a></div>
</div>

Personal salesforce developer environment with custom automation.

- Language: Apex, Bash
- A personal developer environment used for testing configuration and automation
- Makefile - Automated bash scripts for creating scratch orgs, generating components, and deploying to an org
- GitHub Actions - Automatically validate component files and run apex tests when a pull request is created, and deploy when a GitHub release is published
- Apex Trigger Framework - A forked version of a common trigger design pattern that standardizes common use cases for validation, applying default values to records, and bypassing logic