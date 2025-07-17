+++
title = 'Open Source Contributions'
date = 2024-08-23T11:18:58-04:00
weight = 25
description = "Contributions to Open Source Projects"
tags = ['salesforce', 'open-source']
pageName = 'open-source'
icon = 'people'
draft = false
+++

<div style="display: flex; flex-direction: row;">
    <div style="margin-right: 10px;"><a href="https://github.com/k-capehart"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=whitef" alt="Link to Github"></a></div>
</div>

A collection of contributions I have made to open source projects.

## Salesforce Command Line Interface
The official Salesforce CLI. Projects include [salesforcecli](https://github.com/salesforcecli), [oclif](https://github.com/oclif), and [salesforcedx-apex](https://github.com/forcedotcom/salesforcedx-apex).

- [PR#894](https://github.com/oclif/core/pull/894) - Allow standard input to be read as a value for flags
- [PR#886](https://github.com/salesforcecli/plugin-auth/pull/886) - Add `sfdx-url-stdin` flag to allow users to include their SFDX Authorization URL to be piped into the `sf org login sfdx-url` command as standard input
- [PR#873](https://github.com/salesforcecli/plugin-deploy-retrieve/pull/873) - Add `concise` flag to `sf project deploy preview` to suppress ignored files from displaying in terminal output
- [PR#377](https://github.com/forcedotcom/salesforcedx-apex/pull/377) - Reformat terminal output for apex test results
- [PR#504](https://github.com/salesforcecli/plugin-apex/pull/504) - Add `concise` flag to `sf apex run test` to suppress successful test results in terminal output

## Lightning Flow Scanner
A community created plugin that scans Flows in Salesforce for deviations from best practices.

- [PR#83](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-core/pull/83) - Implement new rule to detect flows that are inactive
- [PR#94](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-sfdx/pull/94) - Enable automated testing via GitHub Actions
- [PR#96](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-sfdx/pull/96) - Enable automated dependency updates with Dependabot