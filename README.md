Interactive DNS Resolution Visualizer
An interactive, single-page web visualizer demonstrating the end-to-end lifecycle of resolving a domain name into an IP address.

The application steps through client-side cache checks, local gateway handling, recursive resolver lookups, and iterative queries across Root, TLD, and Authoritative Nameservers—animating the packet flow and querying live DNS-over-HTTPS (DoH) APIs in real time.

Features
Live DoH Lookups: Queries authentic DNS records via Google Public DNS ([https://dns.google/resolve](https://dns.google/resolve)) with an integrated offline fallback for major domains.

RFC 1034 / 1035 Flow Simulation: Visualizes both recursive and iterative query phases across all layers of the DNS hierarchy.

Animated Network Topology: SVG-based network map tracing request and response datagrams with live glowing active node states.

Wire Packet Inspector: Displays simulated packet headers, DNS flags (RD=1, QR=1), query types (A, AAAA), and socket metadata.

Zero Dependencies: Pure HTML5, CSS3, and modern vanilla JavaScript. No frameworks, build steps, or package managers required.

DNS Resolution Flow
The visualizer walks through the following 8-step resolution sequence:

[Browser] ──(1. Cache Miss)──> [OS Resolver / Hosts]
                                      │
                               (2. Cache Miss)
                                      ▼
                             [Home Router / Gateway]
                                      │
                               (3. Cache Miss)
                                      ▼
                           [ISP Recursive Resolver]
                            │         │         │
                   (4. RD=1)│         │         │
                            ▼         ▼         ▼
                      [Root Server] [TLD] [Authoritative NS]
                         (.)        (.com)   (e.g., Akamai/AWS)
                                                │
                                          (5. Live A Record)
                                                ▼
[Browser Socket Connects (TCP/TLS)] <─── [Answer Cached Back]
Browser Cache Check: Inspects browser-level DNS socket cache (chrome://net-internals#dns).

OS & Hosts File: Evaluates local operating system resolution cache (getaddrinfo, Windows dnscache) and static map overrides (/etc/hosts).

Local Gateway / Router: Queries home router or local DNS forwarder cache (e.g., dnsmasq).

ISP Recursive Resolver: Receives the query with the Recursion Desired flag (RD=1) set and checks its regional cache.

Root Nameserver Referral (.): Queries 1 of the 13 Root Server clusters ([a-m].root-servers.net) to locate the delegated TLD registry.

TLD Nameserver Referral: Contacts the Top-Level Domain registry server (e.g., Verisign for .com) for the domain's authoritative nameservers.

Authoritative Nameserver Query: Queries the domain's authoritative nameservers for canonical A and AAAA records.

Answer Propagation & Caching: Traverses back through the resolver hierarchy, populating TTL caches at each level and handing the IP back to the browser to initiate a TCP/TLS handshake.

Getting Started
Local Setup
Clone or download this repository:

Bash


git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
Open index.html directly in any modern browser:

Double-click index.html, or

Start a local HTTP server:

Bash


# Python 3
python3 -m http.server 8080
Then open http://localhost:8080 in your browser.

Deployment to GitHub Pages
Push your repository to GitHub:

Bash
git add index.html README.md
git commit -m "Add DNS visualizer and docs"
git push origin main
Navigate to your repository on GitHub.com.

Go to Settings > Pages (under Code and automation).

Under Build and deployment:

Source: Select Deploy from a branch.

Branch: Select main (or master) and folder /(root).

Click Save.

Your visualizer will be live at:

Plaintext


https://<your-username>.github.io/<your-repo-name>/
Technologies Used
HTML5: Semantic document layout and vector canvas integration.

CSS3: Custom CSS variables, modern flex/grid layouts, keyframe glows, and responsive styling.

Vanilla JavaScript (ES6+): Async/await fetch mechanics, cubic-bezier SVG coordinate interpolation for packet paths.

Google Public DoH API: Real-time JSON DNS lookup endpoints.

License
This project is licensed under the MIT License. Feel free to use, modify, and distribute it for educational or personal use.
