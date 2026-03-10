# LocalSend Code Map

This document outlines the core structure and recent functional updates to the LocalSend codebase.

## Network Discovery & Multicast

Network discovery allows devices to find each other on the local network using UDP Multicast.

*   **`common/lib/constants.dart`**: Contains critical protocol and configuration constants.
    *   `defaultMulticastGroup`: `'224.0.0.167'` (IPv4 Multicast Group).
    *   `defaultMulticastGroupIpv6`: `'ff02::167'` (IPv6 Multicast Group).
*   **`common/lib/src/task/discovery/multicast_discovery.dart`**: Core logic for the `MulticastService`.
    *   `_getSockets()`: Binds non-blocking DatagramSockets on all available network interfaces for both IPv4 and IPv6 to ensure widespread campus and local network compatibility.
    *   `sendAnnouncement()`: Broadcasts the device's presence across all bound IP socket types to both IPv4 (`224.0.0.167`) and IPv6 (`ff02::167`) multicast groups.
    *   `_answerAnnouncement()`: Responds to a discovery broadcast UDP packet from another device, again respecting the IP version of the incoming socket.
*   **`common/lib/api_route_builder.dart`**: Handles the formatting of LocalSend URLs.
    *   `target()` / `targetRaw()`: Generates network URIs safely formatting IPv6 literal addresses using brackets (e.g., `[fe80::1]`).
