---
title: Juice Shop Pentesting Writeups
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


# SQLi — Authentication Bypass

## Target
POST /rest/user/login
Body: {"email":"...", "password":"..."}

## Payload
{"email":"' OR 1=1-- ","password":"x"}

## Why it works
Backend concatenates email into SQL string.
' closes the string, OR 1=1 always true, -- comments rest.
Returns first row in Users table → admin.

## Detection
Server-side logs would show:
- Login request with non-email-format value in email field
- Successful auth without password verification
- Suspicious chars: ' " -- ; in auth fields

## Fix
Parameterised queries. Never concatenate user input into SQL.