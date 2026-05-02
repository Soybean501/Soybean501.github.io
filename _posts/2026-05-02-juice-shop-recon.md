---
title: Juice Shop Recon 1
date: 2026-05-02
excerpt:
tags:
  - by-hand
  - technical
  - pentesting
---

# Juice Shop v19.2.1 Recon

## Stack
- Frontend: Angular 20.3.18 (SPA)
- Backend: Node.js + Express ^4.22.1
- DB: SQL + MongoDB (mixed)
- App path on server: /juice-shop/
- Real-time: Socket.IO

## Notable headers
- No X-Powered-By, no Server (stripped — good)
- No CSP (XSS will execute freely)
- CORS: Access-Control-Allow-Origin: *
- Custom: X-Recruiting: /#/jobs

## Hidden paths
- /ftp/ — directory listing, .md/.pdf whitelist (bypassable)
- Score Board at /#/score-board (hidden Angular route)

## Top 5 targets
[list above]

## Everything else
[paste endpoint list]