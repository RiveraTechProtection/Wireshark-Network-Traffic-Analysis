# Suspicious Outbound Network Traffic Investigation

### Wireshark | DNS | TCP/TLS Analysis | SOC Incident Response

**Analyst:** Marcos Rivera  
**Portfolio:** Rivera Tech Protection  
**Project Type:** Simulated SOC Network Investigation

## Project Overview

This simulated SOC investigation examines suspicious outbound network activity originating from an internal workstation.

The investigation follows DNS resolution, TCP connection establishment, TLS 1.3 negotiation, outbound-heavy encrypted transfers, and periodic connections consistent with possible automated communication.

The objective was to determine whether the observed traffic warranted escalation while distinguishing confirmed packet evidence from potential malicious behavior.

> **Lab Disclaimer:** This is a simulated training investigation using fictional or documentation-reserved network indicators.

## Objectives

- Isolate suspicious host traffic using Wireshark display filters.
- Analyze DNS queries and responses.
- Interpret TCP connection establishment.
- Examine TLS 1.3 communication.
- Compare inbound and outbound traffic volumes.
- Identify periodic or beacon-like connections.
- Develop evidence-based Tier 1 escalation recommendations.

## Tools and Skills

- Wireshark
- DNS Protocol Analysis
- TCP Three-Way Handshake
- TLS 1.3 Traffic Analysis
- Wireshark Conversation Statistics
- Network Traffic Investigation
- SOC Alert Triage and Escalation

## Investigation

### Phase 1: Initial Network Alert

A medium-severity alert identified unusual outbound traffic originating from workstation `192.168.1.45`.

The workstation queried `updates-check.test`, which resolved to `198.51.100.42`.

The host subsequently initiated a TCP connection to destination port 443.

**Wireshark display filter:**

```wireshark
ip.addr == 192.168.1.45
```

The captured packets showed the TCP three-way handshake:

1. SYN
2. SYN/ACK
3. ACK

**Finding:** DNS resolution and TCP connection establishment succeeded. These events alone did not establish malicious activity.

### Phase 2: TLS Session Analysis

The workstation established a TLS 1.3 session with `198.51.100.42`.

Observed packets included:

- TLS Client Hello
- TLS Server Hello
- Encrypted application data in both directions

**Finding:** The capture confirmed encrypted communication, but the application payload could not be directly examined.

Further analysis focused on traffic direction, volume, timing, and subsequent connections.

### Phase 3: Suspicious Outbound Data Transfer

The workstation subsequently queried `cdn-sync.test`, which resolved to `203.0.113.88`.

An encrypted connection to destination port 443 followed.

Wireshark conversation statistics showed:

| Direction | Packets | Bytes |
|---|---:|---:|
| Outbound | 184 | 238,420 |
| Inbound | 26 | 18,991 |

The session contained substantially more outbound traffic than inbound traffic.

**Finding:** The pattern was consistent with an outbound data transfer. However, traffic volume alone did not establish that sensitive information was transmitted.

### Phase 4: Periodicity and Potential Beaconing

Additional connections to `203.0.113.88` occurred at approximately 60-second intervals.

Supporting context included:

- Repeated connections to the same external destination.
- Substantially outbound-heavy encrypted traffic.
- An unknown process initiating the connections.
- No documented approved application using `cdn-sync.test`.
- Only one observed workstation contacting the domain.

**Finding:** The combined evidence was highly suspicious and consistent with possible automated command-and-control or data-exfiltration behavior.

Neither malicious intent nor unauthorized data transfer could be confirmed from the available packet evidence alone.

## Investigation Timeline

| Capture Time | Activity |
|---|---|
| 3.441–3.464 sec | DNS resolution and TCP handshake |
| 3.465–3.478 sec | TLS 1.3 negotiation and encrypted communication |
| 7.102–7.142 sec | Second destination contacted; outbound TLS data observed |
| Conversation analysis | Outbound traffic significantly exceeds inbound traffic |
| ~60-second intervals | Recurring connections to the external destination |

## Incident Assessment

**Severity Assessment:** Highly suspicious activity requiring escalation.

The investigation identified multiple indicators that warranted deeper investigation:

1. Outbound-heavy encrypted communication.
2. Repeated connections at near-regular intervals.
3. Unidentified initiating process.
4. Unverified destination legitimacy.
5. Activity limited to one observed workstation.

These findings supported further investigation into possible command-and-control activity or data exfiltration.

## Escalation Recommendations

1. Preserve the packet capture and investigation timeline.
2. Escalate the activity to Tier 2 SOC or incident response.
3. Identify the process responsible for the outbound connections.
4. Investigate the destination using approved threat-intelligence sources.
5. Review endpoint and proxy telemetry.
6. Determine whether outbound transfers were authorized.
7. Follow organizational containment procedures where appropriate.

## Skills Demonstrated

- Wireshark display filtering
- DNS and domain-to-IP correlation
- TCP handshake analysis
- TLS 1.3 traffic interpretation
- Network conversation statistics
- Inbound/outbound traffic comparison
- Periodic communication analysis
- Evidence-based incident assessment
- SOC investigation documentation and escalation

## Conclusion

This simulated investigation demonstrates a Tier 1 SOC network-analysis workflow.

The evidence confirmed successful DNS resolution, TCP connection establishment, encrypted TLS communication, outbound-heavy data transfers, and recurring connections at approximately 60-second intervals.

The combined findings warranted escalation for additional endpoint, network, and threat-intelligence investigation.

The available evidence did not independently establish confirmed command-and-control activity or data exfiltration.

**Prepared by Marcos Rivera | Rivera Tech Protection**

## Full Investigation Report

📄 [View the Complete Wireshark Investigation](Suspicious%20Outbound%20Network%20Traffic%20Investigation%20%E2%80%94%20Wireshark.pdf)
