# Examples of Consent Records using DPV

> DPV: [https://w3id.org/dpv/](https://w3id.org/dpv/)

**records**

- [`record.json`](record.json): plain JSON format based on current WD5 draft
- [`record_dpv.jsonld`](record_dpv.jsonld): JSON-LD format using DPV
- [`record_dpv.ttl`](record_dpv.ttl): Turtle format using DPV

**receipts**

- [`receipt.json`](receipt.json): plain JSON format based on current WD5 draft
- [`receipt_dpv.jsonld`](receipt_dpv.jsonld): JSON-LD format using DPV
- [`receipt_dpv.ttl`](record_dpv.ttl): Turtle format using DPV

**lifecycle diagrams**

- [`lifecycle.png`](lifecycle.png): diagram showing consent events and lifecycle as states
- [`lifecycle.dot`](lifecycle.dot): source in `dot` (graphviz)
- [editable source - dreampuf.github.io](https://dreampuf.github.io/GraphvizOnline/#digraph%20G%20%7B%0A%20%20%20%20rankdir%3DLR%20%3B%20compound%3Dtrue%20%3B%0A%20%20%20%20node%20%5Bshape%3Drect%2Cstyle%3Dfilled%2Cfillcolor%3Dyellow%5D%20%3B%0A%20%20%20%20start%2Cend%20%5Bshape%3Dcircle%2Cfillcolor%3Dsalmon%2Cwidth%3D0.1%2Cheight%3D0.1%2Cmargin%3D0.01%5D%0A%20%20%20%20start%20-%3E%20NoticeShown%20-%3E%20Requested%20-%3E%20Given%2C%20Refused%20%3B%0A%20%20%20%20Given%20-%3E%20Withdrawn%2C%20Expired%2C%20Invalidated%2C%20Halted%20%3B%0A%20%20%20%20Refused%20-%3E%20NoticeShown%20%5Blabel%3D%22new%20request%22%5D%20%3B%0A%20%20%20%20subgraph%20cluster_T%20%7B%0A%20%20%20%20%20%20%20%20label%3D%22Terminated%22%0A%20%20%20%20%20%20%20%20Withdrawn%2C%20Expired%2C%20Invalidated%2C%20Halted%20%3B%0A%20%20%20%20%7D%0A%20%20%20%20Expired%20-%3E%20end%20%5Bltail%3Dcluster_T%5D%20%3B%0A%20%20%20%20Given%20-%3E%20NoticeShown%20%5Blabel%3D%22re-confirm%22%5D%20%3B%0A%7D%0A)
