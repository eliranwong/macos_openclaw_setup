# OpenClaw Setup for Stock Trading

## Overview

This repository contains personal setup notes and configuration for using OpenClaw on macOS for stock trading purposes. It is intended to serve as backup instructions.

> [!WARNING]
> **Disclaimer:** Use at your own risk. The author takes no responsibility for any actions, financial losses, or outcomes resulting from using these notes.

## Preparations

* [Docker + SearXNG Setup](searxng.md)
* [Coding Agents Setup](coding_agents.md)
* [Discord Setup](discord.md)

## OpenClaw Setup

* [Install](openclaw.md)

## OpenClaw Settings for Multiple Agents Orchestration

Take a look at the [openclaw_sample.json](openclaw_sample.json) file in this repository, and update it with your own configuration. You can use the file as a reference to create your own `openclaw.json` file in `~/.openclaw/`.

## Environment Variables

Take a look at the [zshrc_sample.md](zshrc_sample.md) file in this repository, and update it with your own configuration. You can use the file as a reference to edit your `.zshrc` file in your home directory.

## Restart OpenClaw Gateway

```
openclaw gateway restart
```

## Install Agents Files

Download `workspaces.zip` and unzip it in `~/.openclaw/`. This folder contains the workspaces and agents files for the stock trading project.

Remarks: This zip file is not currently provided in this repository. You may use openclaw to create a set of agents and workspaces that suit your needs.

## Prompt Example for Working with Multiple Agents

In the `stock-coordinator` discord channel, enter:

```
Please instruct the following agents to run a scan in their respective channels. Process them sequentially in the listed order, ensuring each agent finishes its task before initiating the next:

* sector-rotation-scanner
* technical-pattern-scanner
* unusual-options-scanner
* short-squeeze-scanner
* breaking-news-scanner
* second-order-scanner
* 48-hour-surge-scanner
* 3-day-5pct-scanner
* 7-day-explosive-scanner
* operational-milestone-scanner
* ipo-spac-scanner
* pre-surge-scanner
* pre-catalyst-scanner
* pre-move-discovery-scanner
* dip-before-catalyst-scanner
* contrarian-sentiment-scanner
* oversold-bounce-scanner
* base-breakout-scanner
* mega-mover-scanner
* sec-filing-anomaly-scanner
* insider-buying-scanner
* insider-selling-dilution-scanner
* policy-catalyst-scanner
* international-exchanges-scanner
* event-driven-scanner
* analyst-upgrade-downgrade-scanner
* earnings-estimate-revision-scanner
* pre-earnings-runup-scanner
* earnings-post-event-scanner
* crypto-correlation-scanner
* biotech-catalyst-scanner

Do not trigger multiple scans concurrently, as it can overload my device and the LLM. Allow sufficient time for each scan to complete fully before starting the next.
```

## Other Platforms

This repository is for macOS setup only. Read the following for general setup instructions for other operating systems:

* Nvidia Jetson - read https://github.com/eliranwong/NvidiaJetsonOpenClaw
* Linux - read https://github.com/eliranwong/AMD_iGPU_AI_Setup/blob/main/openclaw.md
* ChromeOS - read https://github.com/eliranwong/ChromeOSLinux/blob/main/openclaw.md