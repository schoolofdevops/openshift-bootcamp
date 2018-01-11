# Openshift Bootcamp
---------

|      |      |
| :------------- | :------------- |
| Duration       | 2days     |
| Level     | Intermediate, Advanced |
| Modules          | 10 |
| Flipped Class | No |
| Customizable  | Yes |

## Objectives

This course serves as a accelerator program to understand, setup and master openshift container platform for professionals who already have an understanding of docker and kubernetes.

## Who is this for ?

  * This course is for someone who has already taken docker fundamentals, and kubernetes bootcamp  courses/have equivalent knowledge, and would like to learn how to leverage **Openshift Platform** which is an abstraction on top of it.
  * If you are a **Operations/Systems** personnel and would like to learn how to build a production grade scalable, fault tolerant and high available openshift infrastructure, and administer it,  this course it for you.
  * If you are a **developer** and would like to learn how to deploy your application stacks in production, on top of paas solution and also understand the underlying primitives,  this course is for you.
  * You could be developer/operations personnel and be in charge of securing application infrastructure and setup auxiliary services such as  monitoring, centralized  logging etc. this course is for you.

## Who is this not for ?

  * If you are a **advanced user of Openshift** already, this course is definitely not for you.
  * If you are interested in learning docker/container orchestration on **windows**, this course is not ideal for you as it focuses on linux containers.
  * This is mix course for both developers and operations. If you are looking for a course which is very specific to an audience e.g.  administrators or developers, ask for a custom outline.

## What will you do as part of this course ?

As part of this course you will,  

   * GO through the theory to learn what is openshift platform, the core concepts relates and the advantages of using it.
   * Install and configure a simple(non HA) multi node Openshift cluster  with **ansible**. You would also have a conceptual understanding of  how to build a production quality cluster with high availability, scalability,  redundancy and  security considerations.
   * Learn how to deploy, configure, interconnect and publish and scale applications as well as isolate those with multi tenant environments that openshift provides and underlying kubernetes primitives that it leverages.  
   * Achieve  **Continuous Integration and Delivery** with openshift's integration with git, jenkins and its implicit primitives including builds, pipelines and image streams.  
   * Learn how to manage **persistent storage** in a openshift environment
   * Learn about the network and security considerations and features offered by Openshift.
   * Learn openshift **administration** tasks such as setting up quotas, managing memberships, monitoring etc.

## What is not covered ?

Even though this course covers many concepts related to kubernetes, since its a very vast topic, it still has the following areas uncovered.

  * Cloud specific provisioning and integration
  * HA installation  of a kubernetes cluster with multi masters
  * In depth openshift administration
  * Writing Micro Services Applications
  * Alternate container runtimes e.g. rocker/rkt, runc


## Pre Requisites

Following are the pre requisite skills to attend this course. Since its a beginner level course, no prior experience with linux containers is assumed.  

### Courses

You should have attended the following course, or have demonstrable knowledge with the topics included in the following course.

  * Docker Fundamentals
  * Kubernetes Bootcamp

Pre Assessment test will be conducted at the beginning of the course to asses the skills.

### Skills

  * Docker  
  * Kubernetes
  * Linux/Unix Systems Fundamentals
  * Familiarity with Command Line Interface (**CLI**)
  * Fundamental knowledge of editors on linux (any one of vi/nano/emacs)
  * Understanding of **YAML** syntax and familiarity with reading/writing basic YAML specifications
  * Recommended  to have a basic understanding of Ansible

### Hardware and Software  Requirements

These are the prerequisites for each attendee.

| Hardware Requirements | Software Requirements |
| :---------------------| :--------------------- |
| Laptop/Desktop with high speed internet connection | Base Operating System : Windows / Mac OSX |
| 8 GB RAM | VirtualBox  |
| 4 CPU Cores | Vagrant |
| 20 GB Disk Space available | ConEmu (Windows Only) |
| | Git for Windows (windows only) |
| | minishift |

Lab Setup : Instructions can be found at xxx

## Supporting Content/Materials

Following is the supporting material which will be provided to you before/during the course  

  * Slides (online)  
  * Workshop (online link)  
  * Video Course - XXX by School of Devops  

## Pre Class Checklist

All participants should have completed the following checklist before attending the course .

  * Successfully Completed  **Docker Fundamentals** Course, or have equivalent skills.
  * Successfully Completed  **Kubernetes Fundamentals** Course, or have equivalent skills.
  * Verify  your system meets the  hardware pre requisites.
  * Validate the setup : verify all pre requisite software is installed on your system and is functional.
  * Join our [kubernetes channel on gitter](https://gitter.im/schoolofdevops/Kubernetes?utm_source=share-link&utm_medium=link&utm_campaign=share-link)

## Topics
Following are the topics which would be covered as part of this course. Detailed course outline follows.

  * Introduction to Openshift
  * Openshift Quick Dive
  * Application Lifecycle Management
  * Application Stack Mapping
  * Continuous Integration and Delivery
  * Designing Production Grade Openshift Architecture
  * Setup a Openshift Cluster
  * Securing Openshift
  * Openshift Administration

## Detailed Course Outline
This is the detailed course outline with day wise list of contents

### Day I

#### Introduction and Pre Assessment
  * Trainer, class and course introduction
  * Pre Assessment Test

#### Module 1: Introduction to Openshift
  * What is Openshift?
  * Kubernetes Vs Openshift
  * Key Features
  * Architecture

#### Module 2: Openshift Quick Dive
  * Minishift
  * Setup Minishift
  * Create a Openshift environment
  * Login and validate

#### Module 3: Application Lifecycle Management
  * Projects
  * Types of application deployments
    * Catalogue
    * Image
      * Image stream
      * Bring your own image
    * YAML files

#### Module 4: Application Stack Mapping
  * Deployments
  * Pods
  * Service
  * Routes
  * ConfigMaps
  * Secrets
  * Auto Scaling

#### Module 5: Continuous Integration and Delivery
  * Image streams
  * Builds
  * Git Integration and auto tagging
  * Pipelines and Jenkins integration
  * Automated Delivery
#### Module 6: Designing Production Grade Openshift Architecture
  * Components of Openshift
    * etcd
    * Masters and Nodes
    * Registry
    * Web console
  * Achieving High Availability(HA)
#### Module 7: Setup a Openshift Cluster
  * Types of Openshfit installations
  * Provision nodes
  * Installing Openshift with Ansible
    * Configuring inventory
    * Defining variables
    * Run a playbook
  * Openshift Configuration
    * Master and Node configs
    * Authentication
    * Authorization
  * Network Configuration
    * SDN with OpenVswitch
    * SDN plugins
    * Alternate configs
    * Packet flow
  * Registry Configuration
  * Web Console Configuration
  * Persistent Storage
    * Storage plugins
    * PV, PVC, Storage Class, etc .,

#### Module 8: Securing Openshift
  * Authentication
  * Authorization
    * RBAC
      * Cluster and Local Roles
      * Rules and Bindings
    * SSC
      * Security Context Constraints
    * Security considerations

#### Module 9: Openshift Administration
  * Quota
  * Memberships
  * Monitoring
  * Logging
