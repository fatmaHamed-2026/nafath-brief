Version: NAFATH-V2
Track: Networking

## Overview
This technical report for Mission NAFATH-V2 (Networking Track) provides an initial architectural analysis of 5G Network Slicing for emergency first responders under 3GPP TS 23.501 standards. It outlines high-level resource allocation principles, discusses emergency traffic prioritization, and explores open challenges regarding RAN enforcement across diverse Omani topographies.

Resource: 3GPP TS 23.501, System architecture for the 5G System (5GS) - http://www.3gpp.org/DynaReport/23501.htm

## Software
Safe methods only read data and change nothing on the server: GET, HEAD, OPTIONS, and TRACE. Idempotent methods give the same result whether you send them once or many times. That group includes the safe methods plus PUT and DELETE. Every safe method is idempotent, but not the other way around. POST and PATCH are not idempotent.

This matters for retries. If a connection drops, the client can’t tell whether the server already processed the request. Retrying an idempotent request does no harm, but retrying a POST could create two orders. That’s why non-idempotent requests shouldn’t be retried automatically, unless they carry a unique key that lets the server spot the repeat.

Resource: HTTP support in .NET – Microsoft Learn - https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/http/http-overview
Resource: HTTP Methods – http.dev - https://http.dev/methods
Resource: What Is an Idempotent Request? – APYHub - https://apyhub.com/blog/what-is-an-idempotent-request
Resource: RFC 9110, sections 9.2.1 and 9.2.2 - https://www.rfc-editor.org/rfc/rfc9110.html

## AI
Overfitting happens when a model memorizes noise in the training data as if it were a real pattern, so it performs poorly on new data. A common sign is training performance that keeps improving while validation performance stalls or drops. Cross-validation helps by repeating training and validation across several data partitions, which gives a more reliable estimate of performance. For a congestion prediction model, this makes it more likely to cope with sudden, unseen emergency traffic surges, though never guaranteed.

Resource: Overfitting and the Train/Validation/Test Split – KodeKloud Notes - https://notes.kodekloud.com/docs/Machine-Learning-Fundamentals/Generalisation-and-Types-of-Learning/Overfitting-and-the-TrainValidationTest-Split/page.md
Resource: Prevent Overfitting in Model Training – Pluralsight - https://www.pluralsight.com/courses/prevent-overfitting-model-training
Resource: Machine Learning Crash Course – Google for Developers - https://developers.google.com/machine-learning/crash-course

## Mobile
When first responders lose cellular coverage, the mobile app must maintain seamless usability by storing critical reports locally using a persistent database such as SQLite (via Room). Outgoing requests should be safely queued with clear UI offline feedback. Once connectivity returns, background processes systematically push synced records using exponential backoff retry logic, while timestamps and server-side reconciliation mitigate data conflicts to maintain system integrity.

Resource: Android Mobile App Architecture: Offline First - https://developer.android.com/topic/architecture/data-layer/offline-first

## Networking
5G network slicing creates multiple logical networks on shared physical infrastructure, each tailored to specific latency, bandwidth, and reliability needs. A slice is identified by an S-NSSAI, which the core network uses to select the right slice for each device. A dedicated emergency slice gives responders isolated, prioritized capacity, so voice, video, and location traffic stays stable during congestion, while public traffic continues on separate slices.

Resource: 3GPP TS 23.501, System architecture for the 5G System (5GS) - http://www.3gpp.org/DynaReport/23501.htm

## Cybersecurity
The SANS incident response framework consists of six primary phases: Preparation (establishing defenses and policies), Identification (detecting and analyzing anomalies), Containment (limiting the spread of the attack), Eradication (removing the threat vectors), Recovery (restoring systems securely to normal operations), and Lessons Learned (reviewing post-incident reports). When a 5G service is attacked, Identification flags the intrusion, Containment isolates the compromised slices, Eradication and Recovery restore clean operations, and Lessons Learned harden the network against future threats.

Resource: Incident Handler's Handbook – SANS Institute - https://www.sans.org/white-papers/33392/

## Data
Star schemas feature a central fact table surrounded by denormalized dimension tables joined directly, offering fast read performance for analytical queries. Snowflake schemas normalize dimension tables into multiple sub-tables to reduce redundancy, requiring complex multi-joins. For daily regional 5G service usage reports, a star schema is ideal because it optimizes query speed and simplifies aggregation for project managers reviewing performance metrics.

Resource: Dimensional Modeling Techniques – Kimball Group - https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/

## Misconception in My Track
Misconception: A 5G network slice is a separate physical network built just for emergency responders.

Correct version: A slice instance is defined within the operator’s existing network (PLMN) and consists of core control-plane and user-plane functions plus the access network components serving it. Operators can dedicate a slice to a specific customer with a committed service level.

Proof: 3GPP TS 23.501, clause 5.15.1 “General” (under 5.15 Network slicing).

Resource: 3GPP TS 23.501, System architecture for the 5G System (5GS) - http://www.3gpp.org/DynaReport/23501.htm

## Open Question
How can Omani MNOs dynamically enforce RAN resource isolation and preemption mechanisms for dedicated emergency 5G network slices across rugged topographies, ensuring strict SLA compliance and deterministic low latency during localized congestion?
