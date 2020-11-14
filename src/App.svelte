<script lang="ts">
  import Libp2p from "libp2p";
  import Libp2pWebRTCStar from "libp2p-webrtc-star";
  import Libp2pWebSockets from "libp2p-websockets";
  import Libp2pMplex from "libp2p-mplex";
  import Libp2pBootstrap from "libp2p-bootstrap";
  import Libp2pKadDHT from "libp2p-kad-dht";
  import Libp2pGossipsub from "libp2p-gossipsub";
  import { NOISE as LIBP2P_NOISE } from "libp2p-noise";
  import { onMount } from "svelte";

  onMount(async () => {
    const node = await Libp2p.create({
      addresses: {
        listen: [
          // "/dns4/wrtc-star1.par.dwebops.pub/tcp/443/wss/p2p-webrtc-star",
          // "/dns4/wrtc-star2.sjc.dwebops.pub/tcp/443/wss/p2p-webrtc-star",
          "/dns4/localhost/tcp/3000/ws/p2p-webrtc-star/",
        ],
      },
      modules: {
        transport: [Libp2pWebSockets, Libp2pWebRTCStar],
        connEncryption: [LIBP2P_NOISE],
        streamMuxer: [Libp2pMplex],
        peerDiscovery: [Libp2pBootstrap],
        dht: Libp2pKadDHT,
        pubsub: Libp2pGossipsub,
      },
      config: {
        dht: {
          enabled: true,
          randomWalk: {
            enabled: true,
          },
        },
        peerDiscovery: {
          [Libp2pBootstrap.tag]: {
            enabled: true,
            list: [
              "/dnsaddr/bootstrap.libp2p.io/p2p/bbq",
              // "/dnsaddr/bootstrap.libp2p.io/p2p/QmbLHAnMoJPWSCR5Zhtx6BHJX9KiKNN6tpvbUcqanj75Nb",
              // "/dnsaddr/bootstrap.libp2p.io/p2p/QmZa1sAxajnQjVM8WjWXoMbmPd7NsWhfKsPkErzpm9wGkp",
              // "/dnsaddr/bootstrap.libp2p.io/p2p/QmQCU2EcMqAqQPR2i9bChDtGNJchTbq5TbXJJ16u19uLTa",
              // "/dnsaddr/bootstrap.libp2p.io/p2p/QmcZf59bWwK5XFi76CZX8cbJ4BhTzzA3gU1ZjYZcYW3dwt",
            ],
          },
        },
      },
    });

    node.on("peer:discovery", (peerId) => {
      console.log(`Found peer ${peerId.toB58String()}`);
    });
    node.connectionManager.on("peer:connect", (connection) => {
      console.log(`Connected to ${connection.remotePeer.toB58String()}`);
      console.log(connection.remotePeer);
    });
    node.connectionManager.on("peer:disconnect", (connection) => {
      console.log(`Disconnected from ${connection.remotePeer.toB58String()}`);
    });

    await node.start();
    console.log(`libp2p id is ${node.peerId.toB58String()}`);
  });
</script>

<style lang="scss">
</style>

<div class="d-flex main-container">Hello world</div>
