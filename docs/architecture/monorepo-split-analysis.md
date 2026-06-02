# Monorepo Split Analysis

## Overview
The goal is to decouple the legacy Java EE monolith into independent services and frontends.

## Identified Hotspots
- CustomerOrderServices: Shared JPA model.
- Kafka topic-item-ordered: Integration point.

## Split Strategy
Strangler Fig pattern for the Service Catalog, followed by Micro-frontend architecture for the UI.