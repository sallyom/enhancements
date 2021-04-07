---
title: distributed-tracing-with-opentelemetry
authors:
  - "@sallyom"
  - "@parulsingh"
reviewers:
  - TBD
approvers:
  - TBD
creation-date: 2021-04-14
last-updated: 2021-06-09
status: informational
---

# Distributed Tracing with OpenTelemetry

## Release Signoff Checklist

- [ ] Enhancement is `implementable`
- [ ] Design details are appropriately documented from clear requirements
- [ ] Test plan is defined
- [ ] Operational readiness criteria is defined
- [ ] Graduation criteria for dev preview, tech preview, GA
- [ ] User-facing documentation is created in [openshift-docs](https://github.com/openshift/openshift-docs/)

## Summary

This document is an overview of distributed tracing and how it benefits a distributed system
such as OpenShift. Also, this document provides information necessary
to configure distributed tracing in an OpenShift deployment with a vendor-agnostic collector
capable of exporting telemetry data to any back-end analysis tool. Some open-source back-ends
for OpenTelemetry data are Jaeger, Prometheus, and Zipkin. Here is a list of [vendors that currently support
OpenTelemetry.](https://opentelemetry.io/vendors/)

Distributed tracing has the ability to quickly detect and diagnose problems as well
as improve performance of distributed systems and  microservices. By definition,
distributed tracing tracks and observes service requests as they flow through a system by
collecting data as requests go from one service to another. It's possible, then, to
pinpoint bugs and bottlenecks or other issues that can impact overall system performance.
Simply put, tracing provides the story of an end-to-end request that is difficult to get otherwise.

OpenTelemetry supports a wire protocol `OTLP` over `gRPC`. `OTLP` is a request/response protocol. `OTLP`
is used to send data to an `OpenTelemetry Collector` that can be configured to forward and export
data to any data analysis tool that supports OpenTelemetry.

## Motivation

As platforms and applications become more distributed and built on microservices or serverless, tracing
provides an overall picture of system performance. This visibility reveals service dependencies and how
one component affects another, things which are difficult to observe otherwise. For example, many OpenShift
bugs or issues are not contained to a single component. Instead, several teams and component owners often
work together to solve issues and make system improvements. Distributed tracing aids this by
tracking events across service boundaries. Furthermore, tracing can shrink the time it takes to diagnose issues,
giving useful information and pinpointing problems without the need for extra code. Upstream, etcd has been
instrumented to export gRPC traces. CRI-O is also adding instrumentation. There is an open PR with
kubernetes-apiserver that will add opentelemetry tracing, and work has been underway to instrument kubelet
and kube-scheduler as a POC. With these components instrumented, it will be possible to view traces with
CRI-O <-> Kubelet <-> Kube-Scheduler <-> Kube-Apiserver <-> ETCD. At this point, there is much to gain in instrumenting
other components and extenting the opentelemetry train to give a complete view of the system.

### Goals

Provide an easy way for OpenShift components to add instrumentation for distributed tracing using OpenTelemetry.
Adding opentelemetry tracing spans requires only a few lines of code. The more components that add tracing,
the more complete the picture will be for anyone who is debugging or trying to understand cluster performance.
Also, a vendor-agnostic OpenTelemetry operator available through the OpenShift operator hub can
be temporarily deployed in times of debugging to turn on tracing, and removed when no longer needed. Any component
that adds instrumentation should add a noop exporter and/or a switch to turn tracing on. It should be easy to enable
and disable tracing. When disabled, there will be no instrumentation. If enabled but no backend is detected, the
instrumentation will be removed, so that there will be no trace exporter connection errors in component logs.

### Non-Goals

The OpenTelemetry operator for OpenShift will not be part of core OpenShift. Instead, the operator is available
on the OperatorHub in the OpenShift Console.

## Proposal

This will be a living document that will be a record of adding tracing to CRI-O. The end
result will be a merged document that will serve as a guideline for anyone else who wishes to add tracing
to their components or applications.

### User Stories

* As a cluster administrator, I want to easily switch on or off opentelemetry tracing.
* As a cluster administrator, I want to diagnose performance issues with my component or service (CRI-O).
* As a cluster administrator, I want to inspect the service boundary between CRI-O and kubelet.
* As a component owner, I want to instrument code to enable opentelemetry tracing.
* As a component owner, I want to propagate opentelemetry data to other components.

### Implementation Details/Notes/Constraints [optional]

TODO: Add all steps required to add tracing to a component and how to implement a back-end such as
Jaeger, that is currently available on the OperatorHub in the OpenShift Console.

### Risks and Mitigations

TODO

## Design Details

TODO

### Open Questions [optional]

TODO

### Upgrade / Downgrade Strategy

TODO

## Implementation History

Major milestones in the life cycle of a proposal should be tracked in `Implementation
History`.

## Drawbacks

The idea is to find the best form of an argument why this enhancement should _not_ be implemented.
