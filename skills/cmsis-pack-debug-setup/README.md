# CMSIS-Pack Debug Setup

Coordinates the complete CMSIS-Pack debug setup workflow for an existing DFP PDSC.

Use it when a task spans debug connection description, device-specific non-trace sequences, and CoreSight trace setup. It runs the Debug Description skill first, adds Debug Sequences only when required, then adds Trace Sequences when trace is in scope.

The orchestration skill maintains ownership boundaries between the specialist skills and gathers one combined user confirmation before applying the complete PDSC update.

