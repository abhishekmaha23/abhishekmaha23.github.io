---
title: "Don't like the Internet? What can you do?"
date: 2023-12-24T15:18:29+01:00
draft: false
author: "Abhishek Mahadevan Raju"
layout: "post"
---

<!-- // Personal opinion post, perhaps  -->

The Internet is experiencing (perhaps facilitating?) an inexplicable amount of turmoil in the world now. Never before in history did we ever have such a platform that democratized society at such rates, and to be fair, this was indeed the goal for the whole infrastructure. With the lowering of barriers to all manners of free speech, we've enabled *all* forms of free speech. In a world with tons of options for soapboxes and platforms for the most radical and polarizing views, platforms with the simplest and most addictive presence seem to have won out. We've needed a major rethinking of the internet for a while now, and this hasn't gone unnoticed by most security and internet researchers.

I'm listing out some (non-comprehensive) perspectives I've found to be actively developing/exploring potential changes to the Internet. Contributors wishing to help change the nature of the Internet may find these a good starting point to join existing initiatives, meet like-minded collaborators or even pitch their own ideas:

1. **New (next-gen) internet initiatives.** - Changing the technical foundations of Internet communication.

    The Internet as-is is an ecosystem of addresses and communication. Two directions are emerging with some urgency to upgrade the robustness of our infrastructure and communities. I've observed the following perspectives, which are perhaps intertwined:
    - An identity layer for the internet - Self-Sovereign Identity - [here](https://livebook.manning.com/book/self-sovereign-identity/chapter-1/v-1/)
    
        Technically, this involves concepts of introducing digital identities (through Decentralized Identifiers) and credentials (through Verifiable Credentials), and supporting standards for various protocols of communication (i.e. for authentication, messaging and other forms of message-passing). At a human level, this might mean setting up trusted issuers of credentials (such as governments, universities or other non-profit trusted organizations issuing narrowly scoped digital identifiers) to handle the governance of this new decentralized ecosystem.

    - A trust layer for the internet [here](https://www.cira.ca/en/resources/documents/state-of-internet/a-trust-layer-for-the-internet-is-emerging-a-2023-report/)

        The Trust over IP Foundation is developing a reimagination of our internet through the Trust Spanning Protocol (see [here](https://volodymyrpavlyshyn.medium.com/trust-over-ip-architecture-review-trust-spanning-protocol-67bd4a1ee163) and [here](https://trustoverip.org/news/2022/11/14/toip-tech-arch-first-public-review/)). This could be quite ambitious, but is also equally promising. 
    
    These ecosystems and developments aren't new. Organizations like [Next Generation Internet](https://www.ngi.eu/) and [NLNet](https://nlnet.nl/project/current.html) are willing to fund new ideas that may pitch interesting new and different ideas for our Internet, or even additions to our current one! There's numerous open calls for proposals, and they offer funding in significant ways. Similarly, conferences like Splash [Onward](https://2023.splashcon.org/track/splash-2023-Onward-papers), and [UbiComp](https://www.ubicomp.org/ubicomp-iswc-2023/) may be interesting to contributors looking for platforms for exploring new ideas! 


2. **Decentralization approaches** - Changing the architectural foundations of how services are offered to users.

    - *Fediverse*
    
        Purely in the context of social media, and communities, the ripple effects of centralization are hard to ignore. One prominent direction to change the form of applications we spend our time on is through the Fediverse - which involves developing smaller modular communities to replace larger ones. Initial results are promising! Networks such as [BlueSky](https://bsky.app/) and [Mastodon](https://mastodon.social/explore) are the most prominent (with slightly different architectural designs), but are yet to reach the scale of the centralized alternatives. But it's a start!

        Other forms of federations such as through Matrix servers are still niche, and the barrier to entry is still too high for non-technical users.

        Other examples of Federated communities: Pleroma, PixelFed, Lemmy, Peertube.

    - *Blockchain, Web3 and Distributed Consensus.*

        For unfortunate-but-expectable reasons, the hype for Web3, NFTs, DeFi and cryptocurrencies has worked against the reasonable and logical foundation - a potentially incorruptible distributed ledger.
        
        Applications such as DAOs like [Aragon](https://aragon.org/) are still being actively developed, and have great promise in being integrated in modern societal governance structures!

    - *P2P initiatives*

        Such initiatives are numerous, yet have great interest within their niche communities. Examples include:
        - IPFS
        - Ceramic data network
        - Invisible Internet protocol
        - The BitTorrent protocol
        - Tor
        - ZeroNet
        - Dat Foundation -> Hypercore Protocol -> Holepunch ecosystem
    
    There are an infinite variations on how architectures could be built atop the Internet to provide services to end-users. It is the greatest validation of human ingenuity that we have left so few ideas still unexplored. Services like Matrix can use a lot of love! Self-hosting is proving to be a strong niche submarket, with NAS hardware available at significantly cheaper costs, and a welcoming community at [r/selfhosted](https://www.reddit.com/r/selfhosted/) to give beginners an introduction to the world of digital sovereignty.

3. **Standards Development Organizations (SDOs)** - Publishing standards and moving the Internet forward, one step at a time

    [Technical Standards Development Organizations](https://en.wikipedia.org/wiki/Standards_organization#International_standards_organizations) number in the thousands, with multiple consortia, working groups and foundations in almost every technical domain - with the goal of achieving interoperability and integration across partners and even competitors.

    In the area of the Internet alone, SDOs play a specifically prominent role in not just supporting and extending, but also in faciliating radical future development in a collaborative manner. Examples like:

    - Internet Engineering Task Force
    - World Wide Web Consortium
    - Internet Research Task Force
    - Decentralized Identity Foundation (newer)
    - Trust over IP Foundation (newer)

    Such initiatives provide a working ground for contributors to pitch Internet draft submissions, which then go on through numerous iterations to become RFCs (Request For Comments). The process might take years! For example, check out the page for the drafts currently submitted to the IETF [here](https://datatracker.ietf.org/doc/recent).

    Some examples of fun RFCs!:
    - A Standard for the Transmission of IP Datagrams on Avian Carriers - https://datatracker.ietf.org/doc/rfc1149
    - IP over Avian Carriers with Quality of Service - https://datatracker.ietf.org/doc/rfc2549
    - Adaptation of RFC 1149 for IPv6 - https://datatracker.ietf.org/doc/rfc6214
    - Scenic Routing for IPv6 - https://datatracker.ietf.org/doc/rfc7511

    But every so often you get some interesting and profound ones:
    - The Twelve Networking Truths - https://datatracker.ietf.org/doc/rfc1925/
    - The Internet is for End Users - https://datatracker.ietf.org/doc/rfc8890/
    - Centralization, Decentralization, and Internet Standards - https://datatracker.ietf.org/doc/rfc9518/

    In essence, SDOs work constantly to develop newer standards for authentication and general improvements (such as SSL, better signature standards, Certificate Authorities), publishing specification updates that get picked up by browsers, vendors, end-user clients and content-providers, with ample feedback and input provided throughout the process.

    Broader standards organizations such as ISO, CEN/CENELEC, NIST and others also publish domain-specific standards in these areas - as before, publishing a standard isn't the key part, it's the adoption and maturity of the standard that make a difference, and there may be political factors at play there. 
    
    Contributors may consider joining one of these organizations as an observer to get the hang of the decentralized distributed process (usually such groups have regular/weekly/biweekly meetings where they discuss progress and developments) and publishing ideas and collaborating with potential stakeholders once they identify suitable problem areas. 

4. **Policy directions** 

    Legislative requirements to advance the Internet are perhaps the most difficult to describe in detail, due to the variation across countries, jurisdictions, application domains. In essence, such initiatives focus on the business and end-user impact. Examples of regulations of interest for service providers on the Internet might be towards equitable marketplaces, general data privacy requirements, cryptographic requirements, stricter regulations on financial and healthcare data, and many others. We know how much the GDPR has impacted the world (cookie-banners everywhere), but do we know that this is actually being [enforced](https://www.enforcementtracker.com/) in the EU? (It's not a lot at the moment - the top 2 fines are 1.2 billion EUR to Meta, and 746 million EUR to Amazon. Perhaps too small, but it's a start! Regulations can work if they have sufficient teeth. Would we rather have no regulation or some regulation?)

    A key point of interest here is to take part in the work of Advisory Councils, think-tanks and non-profits that advise policy-makers. Lawmakers are not subject experts, but they do have incentives and mandates, and organizations regularly publish whitepapers and opinion documents that spark debates and discussions on upcoming legislation and decisions to be made. This is indeed an integral part of the process, and perhaps the best way to have a strong impact!
 

I'm missing a lot of the lower-level Internet improvements such as 5G-6G or the IPv6 migration (not as successful yet), and I stick to Layer 7 improvement initiatives for the Internet. But I argue that these systems are directly relevant and impactful to the end-users! We might not care too much about how the internet reaches our house, but we care about online radicalization, misinformation, privacy violations, and perhaps I hope I can help increase awareness of how motivated people can get started in the first place.