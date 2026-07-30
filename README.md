# AutoVisit

> Personal project. Not affiliated with NARSA or the Moroccan Ministry of Transport.
> **All data in this project is synthetically generated. No real inspection data is used.**

## Problem

Vehicle inspection centers manage bookings by phone and paper registers, leading to
infeasible appointments (equipment miscalibrated for the requested date, wrong
technician qualification), lost counter-visit deadlines, and inspection reports
owners cannot understand.

## Architecture

3-tier monolith, Package by Layer (`controllers` / `services` / `repositories` / `models`).
See `docs/adr/` for architecture decisions.

## Stack

Java 21, Spring Boot 3, PostgreSQL, MongoDB, Redis, Spring AI + Ollama (RAG + MCP agent),
React + TypeScript.

## Running locally

[à compléter au ticket AV-14]

## Status

🚧 Work in progress — Sprint 0
