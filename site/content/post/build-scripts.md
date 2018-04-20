---
title: Build Scripts
draft: true
date: '2018-04-20T08:58:37-05:00'
---
Benefits
- Debug locally
- Reuse build definitions or steps across projects (common build steps in a nuget package)
- Version control
- Cohesion (build defn lives in same repo as project)
- Independence from build tool (TFS/TeamCity/etc), can move easily, can build a hotfix locally if absolutely necessary
- Easier upgrade path (TFS 2013 to 2015 was a nightmare since build system changed)

Downsides
- More difficult to learn (GUI tools are easy to use)
- DSL tools/plugin dependence (some tools don't keep up with build tool version)
- Support (Major tools like TFS and TeamCity have fewer issues and dedicated support teams)

.NET Build Script Languages
- psake
- Cake
- Fake

.NET Build Serers
- Team Foundation Server (TFS)
- TeamCity
- Appveyor
- TravisCI
- CircleCI
- Many others
