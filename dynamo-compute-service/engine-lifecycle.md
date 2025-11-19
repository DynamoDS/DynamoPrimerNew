# The Dynamo Compute Service Lifecycle & Update Cadence

> **Note: Beta Service**  
> Dynamo Cloud Compute is currently in beta. The support timelines and update policies described in this document represent our current intentions as we experiment with and refine the service. These are not guarantees and may change based on user feedback and operational experience.

This document describes the update cadence and support policy for Dynamo Cloud Compute. It may also be referred to in this document interchangeably as "the service".

It outlines how engine versions are managed, when updates occur, and what users can expect when running Dynamo graphs in the cloud.

---

## Update Cadence

To meet different user needs, the Dynamo Cloud Compute maintains **two distinct engine tracks**. Each track serves a specific purpose and follows its own update schedule:

### Stable Engine (Production)

The stable engine is designed for reliability and consistency in production environments. It is based on the latest stable release of the DynamoCore Runtime and is updated when official Dynamo releases are accessible to Dynamo desktop users. Initially we'll be following the update cadence of DynamoRevit.

This track is intended for production workloads where reliability and predictability are critical. When you use the stable engine, you can expect updates to align with Dynamo's public release schedule, giving you time to prepare for changes and test your graphs before they affect your workflows.

### Preview Engine (Preview / Daily Sandbox)

The preview engine provides early access to the latest developments in Dynamo. It is based on the latest development build of the DynamoCore Runtime and is updated frequently as new features and bug fixes are merged.

This track is ideal for users who want to test upcoming changes, experiment with new features before they're officially released, or verify that their graphs will continue to work with future versions of Dynamo. The preview engine allows you to stay ahead of changes and provide feedback to the Dynamo team.

---


## Support Timeline

Understanding how long each engine version remains supported helps you plan maintenance windows and graph updates.

### Stable Engine

The stable engine receives updates when Dynamo publishes a new stable release in DynamoRevit. Each stable version remains available and supported until the next stable release is deployed to the service.

For example, if the service is currently running Dynamo 3.6 (stable), it will continue to run that version until Dynamo 4.0 becomes generally available to users (typically when it ships in Revit). At that point, the service will be updated to Dynamo 4.0 (stable).

This approach ensures that the cloud service stays in sync with what the majority of users experience in desktop environments.

### Preview Engine

The preview engine is continuously updated from the latest development branch of Dynamo. As development progresses on each version, the preview engine tracks those changes.

For example, while Dynamo 4.1 is in active development, the preview engine might be labeled as "Dynamo Cloud Compute Service 4.1". Once development shifts to version 4.2, the preview engine will begin tracking those changes and may be relabeled as "Dynamo Cloud Compute Service 4.2".

Because the preview engine updates frequently, you should expect occasional breaking changes or experimental features. It's best used for testing and validation rather than production workflows.

---

## Choosing the Right Engine

When deciding which engine track to use:

- **Choose stable** if you need predictable, tested behavior for production workflows, or if you're deploying graphs to end users who expect consistent results.

- **Choose preview** if you want to test new features early, validate that your graphs will work with upcoming versions, or contribute feedback on Dynamo development.

Both engines run the same core Dynamo runtime, the difference is when and how often they receive updates. 

---





