---
layout: post
title:  "Security and Microservices by Sam Newman"
date:   2017-08-19 13:00:00 +0100
categories: jekyll update
---

[Security in Microservices](https://www.youtube.com/watch?v=ZXGaC3GR3zU)

A Microservices system is a model of a *business domain* that has been broken into small *autonomous* services that *work together*. *Autonomous* means each service is independently deployable. Developing a system using microservices over a monolithic architecture will increase the number of attack surface vectors it contain, basically you will have more network calls and processes.

## Areas of interest

Prevention -> Detection -> Response -> Recovery

* Prevention - How do you prevent an attack.
* Detection  - How do you know you are being or have been attacked.
* Response   - How do you respond to an attack.
* Recovery   - Once the attack is over how do you recover from that attack.


## Prevention

### Threat modeling

Threat modeling is a procedure for optimizing security by identifying objectives and vulnerabilities, and then defining countermeasures to prevent, or mitigate the effects of, threats to the system.  The idea is not to make it impossible to penetrate just to make it cost prohibitive to do so.

* [Schneier attack trees](https://www.schneier.com/academic/archives/1999/12/attack_trees.html)
* [Microsoft threat modeling](https://msdn.microsoft.com/en-us/library/ff648644.aspx?f=255&MSPPError=-2147217396)

### Transport Security

* Secure channels everywhere.
  * Encrypted communication should be used between all services.

* Https.
  * Server guarantees ( We know who we are talking to ).
  * Payload protection ( We know the payload has not been manipulated ).
  * No client guarantees.
  * [letsencrypt](https://letsencrypt.org/).
    * A free, automated certificate authority.
    * Works well with the automated deployment of services.

* Client side certificates.
  * Client guarantees ( difficult to manage ).
  * low automation.
    * [Lemur ( Netflix )](https://github.com/Netflix/lemur).
  * When crossing the perimeter.

#### Concepts

* Authentication
  * Are you who you say you are.
  * Single service for the entire system ( OAuth ).

* Authorization
  * Are you allowed to do the thing you want to do.
  * Per service decision.

### Security Problems

* [Confused Deputy](https://en.wikipedia.org/wiki/Confused_deputy_problem)
  * Trick an intermediate party into asking for thing they should not be able to get.
  * Mitigate by passing the authentication token between internal service.


### Data at Rest
  * Encryption at the data layer.
    * Affects Performance.
    * Be selective which services are encrypted.
    * Need to think about where keys are stored.
        * Use an key store.

### Docker
  * Docker hub.
    * Images are downloaded from the docker hub.
    * Official images.
      * Have been verified that person who uploaded an image is who they say they are.
      * *No guarantee about if the image is safe and secure*.
      * Docker images are immutable, so can not be patched.
      * Build your own images.
    * Only use your images that you have built and store in your own registry.
    * Images should be scanned inside the build pipeline.
      * [CLAIR](https://github.com/coreos/clair) can scan images as part of the build pipeline.

  * Patch and re-issue.
    * Software to check for unpatched version ( e.g. Upguard ).
    * Patch and re-issue once a week.


## Detection

* It is not just as it happens.
  * It is hard to detect every attack as it happens.
  * You want to be able to detect if you were attacked retrospectively.
* Identify your vulnerability.
  * Automate checking for new versions.
  * Include in the build pipeline.
* Logs should be aggregated and stored remotely.
* The polygot nature of microservices increases exposure.
  * more stuff to track.
  * more surface area to attack.


## Response

* Communicate the fact that it has occurred and what has occurred.
  * Protect your integrity.
    * Be accurate about the extend of a breach and any data loss.
    * Do not extend the public exposure by making false denials.
    * Have empathy for the people effected by a data breach.
    * NOTE - You need to have ways of identifying what has been lost.

* Have pre decided responses for multiple scenarios.
  * What is the wording of the response.
  * Who the response comes from.
  * How the response is communicated ( email, website, etc ).
  * These things are hard to decided in the emotional aftermath of an attack.


## Recovery

* Backup and restore.
  * Have backups.
  * Know how to restore and prove it works regularly.
  * Burn down compromised infrastructure.
  * Automation makes this simple.
* Post mortem.
  * Perform for all issue.
  * Communicate the lessons learnt.
  * Review periodically.


* **It is generally the system that failed rather than an individual.**
