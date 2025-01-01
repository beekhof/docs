---
layout: default
title: FAQ
nav_order: 9
---

# Frequently Asked Questions
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## How do I pronounce Medik8s?

Medik8s is intended to be a playful misspelling of the English word "medicates" and is pronounced the same way.

## Does Medik8s require OpenShift?

No.  Medik8s can run on any kubernetes cluster.

## Does Medik8s require Machine API?

No.  Medik8s puts Nodes at the center of failure detection and recovery and can
run on any kubernetes cluster.

## Does Medik8s require special hardware?

No.  While Medik8s can take advantage of hardware watchdogs and/or BMCs, it also
has options for shared-nothing recovery.

## Does Medik8s work on bare metal only?
No. Medik8s operators can work on any platform, unless specified otherwise by a specific remediator.

## Do all nodes need to be treated the same?

No.  The Node Healthcheck configuration includes a node selector, so you can
treat the control plane differently to workers, and have pools of workers with
different conditions and thresholds to provide a variety of SLAs.

## Can I create my own definition of what counts as a healthy node?

Yes.  Node Healthcheck determines node health based on [NodeConditions](https://kubernetes.io/docs/concepts/architecture/nodes/#condition).  There
are a set of basic conditions built into Kubernetes, but additional conditions
can be defined and then referenced by Node Healthcheck.  Node Problem Detector
is a common tool for creating and updating NodeConditions based on log scraping.

## Can I create my mechanism for recovering a node?

Yes.  Node Healthcheck uses the sig-cluster's [External Remediation API](https://github.com/kubernetes-sigs/cluster-api/blob/main/docs/proposals/20191030-machine-health-checking.md#external-remediation)
to uniquely associate a node failure with a specific recovery mechanism of 
your choosing. 

## How can I get involved?

Reach out to the [google group](https://groups.google.com/g/medik8s) or submit 
a PR to one of the [Medik8s projects](https://github.com/medik8s/).

## What is the relationships to sig-cluster?

The Medik8s team has worked with the [sig-cluster
community](https://github.com/kubernetes/community/tree/master/sig-cluster-lifecycle)
for many years.  While we have many things in common, they are naturally
focussed on furthering the Machine/Cluster APIs.  Basing our solution on those
APIs would limit the types of clusters we can provide a solution for.

## What is the Relationships to Cluster/Machine API?

The original implementation put Machines at the center of failure detection and
exclusively used the [Machine
API](https://github.com/kubernetes-sigs/cluster-api/blob/HEAD/docs/proposals/20181121-machine-api.md)
for recovery.  Node Healthcheck Controller can use the Machine API if it is
available, but also supports other mechanisms.

## What is the connection to Machine Healthcheck Controller?

Apart from the Medik8s team being actively involved with the [Machine
Healthcheck
Controller](https://github.com/kubernetes-sigs/cluster-api/blob/main/internal/controllers/machinehealthcheck/machinehealthcheck_controller.go),
the [Node Healthcheck
Controller](https://github.com/medik8s/node-healthcheck-operator) also shares
much of the same code.

The primary difference between the two implementations is putting Nodes at the
center of failure detection to avoid a dependency on `Machine` objects, which
are not common to all kubernetes installations.

## What is the Relationships to External Remediation API?

The original MHC implementation assumed that using the Machine API to destroy
the bad node and replace it with a new one was the only necessary recovery
mechanism.

The Medik8s team partnered with Ericsson to convince the sig-cluster community
that other mechanisms were needed (particularly on bare metal). Together we
created the External Remediation API that is used by both the Machine and Node
Healthcheck Controllers.

## Is a company behind this?

The Medik8s team is employed at Red Hat, where we leverage 20 years of personal
experience creating HA architectures to create a kubernetes-native HA experience for
workloads such as Stateful sets and RWO Volumes.
