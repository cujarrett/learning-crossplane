# Learning Crossplane

A hands-on learning path for building Crossplane custom APIs from scratch — using **minikube on macOS**, **no cloud provider required**, and **Go templating** to generate Kubernetes resources.

All work is local: write YAML and Go templates, apply them to minikube, test, iterate.

---

## The Local Development Loop

Every chapter follows the same rhythm:

```
  Write (XRD, Composition, or Go template)
          │
          ▼
  kubectl apply -f <file.yaml>
          │
          ▼
  Crossplane reconciles
          │
          ▼
  kubectl get / describe / logs
          │
          └─ tweak and repeat
```

No CI/CD, no cloud. Crossplane runs as pods inside minikube and creates standard Kubernetes resources (Deployments, Services, ConfigMaps). By Chapter 09 you will write your own Composition Function in Go — a real gRPC service that Crossplane calls to render resources.

---

## What You Will Build, Chapter by Chapter

| Chapter | Hands-On Output |
|---------|----------------|
| 01 | Crossplane running on minikube; starter `App` XRD deployed and tested |
| 02 | Deployment + Service applied manually so the automation in later chapters feels concrete |
| 03 | A `WebService` XRD with schema validation, defaults, enum constraints, and status fields |
| 04 | A Composition that turns a `WebService` into Deployment + Service + ConfigMap using Patch & Transform |
| 05 | That same Composition rewritten using Go templates (`function-go-templating`) |
| 06 | Composition Revisions — pin an XR to a specific Composition version; test safe rollout |
| 07 | Namespace isolation and RBAC — developers can only touch their own XRs |
| 08 | A `MicroService` XRD with optional HPA using Go template conditionals and nil-safe patterns |
| 09 | Write a custom Composition Function in Go — your own gRPC function, built and tested locally |

---

## Chapters

| # | Title | Key Concepts |
|---|-------|-------------|
| [01](chapters/01-setup-and-big-picture.md) | Setup & The Local Workflow | minikube, Helm, Crossplane install, starter project tour |
| [02](chapters/02-kubernetes-refresher.md) | Kubernetes Resources Refresher | GVK, CRDs, Deployments, Services, labels |
| [03](chapters/03-xrds.md) | XRDs — Composite Resource Definitions | XRD schema, versions, defaultCompositionRef, spec vs status |
| [04](chapters/04-compositions.md) | Compositions & Go Templating | Pipeline mode, how Functions work (gRPC protocol), P&T as a read-once reference, first Go template hands-on |
| [05](chapters/05-go-templating.md) | Go Templating Deep Dive | Sprig helpers, nil-safe `default dict`, status writeback, `define`/`include` blocks, conditional HPA |
| [06](chapters/06-composition-revisions.md) | Composition Revisions | CompositionRevision objects, Automatic vs Manual update policy |
| [07](chapters/07-providers.md) | Providers & Managed Resources | Upbound provider model, `provider-github`, ProviderConfig, direct MRs, `BugReport` XRD |
| [08](chapters/08-claims-and-rbac.md) | Namespace Isolation & RBAC | Namespaced XRs, Roles, RoleBindings, `kubectl auth can-i` |
| [09](chapters/09-advanced-go-templating.md) | Advanced Go Templating | HPA conditionals, nil-safe patterns, loops, MicroService XRD |
| [10](chapters/10-write-function-in-go.md) | Write a Composition Function in Go | Custom Go function, RunFunction handler, local image load |

---

## Prerequisites

- **macOS** with [Homebrew](https://brew.sh) installed
- **Docker Desktop** running (minikube uses it as the driver)
- **Go familiarity** — your `~/Developer/learning-go/101` work is directly applicable to Chapter 04 onwards
- Basic `kubectl` knowledge (`get`, `apply`, `describe`, `logs`)

---

## The Starter Project

The YAML files in the root of this repo are a minimal working example you deploy in Chapter 01:

| File | What It Does |
|------|-------------|
| `xrd.yaml` | Defines the `App` custom resource API with a `spec.image` field |
| `composition.yaml` | When an `App` is created, create a Deployment + Service |
| `function.yaml` | Installs the `function-patch-and-transform` Crossplane plugin |
| `app.yaml` | An instance of `App` — creates an nginx Deployment + Service |

---

## How To Use This Guide

Each chapter follows a consistent pattern:

1. **Read** — work through the theory top to bottom (concepts, diagrams, examples)
2. **Hands-On** — follow the numbered steps, copy-paste the YAML, run the `kubectl` commands
3. **Test** — every hands-on ends with verification commands so you can see it working

Create your practice files under `practice/` as you go:

```
practice/
  ch01/    ← files you apply in Chapter 01
  ch02/    ← files you apply in Chapter 02
  ch03/
  ...
```

Work through chapters in order — each one builds on the previous.

---

## Quick Reference Commands

```bash
# Cluster lifecycle
minikube start --profile crossplane
minikube stop  --profile crossplane
minikube delete --profile crossplane

# Check Crossplane is healthy
kubectl get pods -n crossplane-system

# What XRDs (custom APIs) are installed?
kubectl get xrds

# What Functions (plugins) are installed?
kubectl get functions.pkg.crossplane.io

# List all composite resources (XRs) in the cluster
kubectl get composite

# Watch resources reconcile in real time
kubectl get deployments --watch

# Debug a specific XR
kubectl describe webservice my-webservice -n default

# See events sorted by time (great for debugging)
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

Start with [Chapter 01 →](chapters/01-setup-and-big-picture.md)
