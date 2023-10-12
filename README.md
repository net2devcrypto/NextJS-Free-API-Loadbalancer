# NextJS-Free-API-Loadbalancer
A very simple way of determining user's geolocation and redirecting them to their closest destination. No CDN nor cloud loadbalancing service required.

<h3>Video Tutorial</h3>

Click for video:

<a href="https://youtu.be/bNeTJgbJ3_A" target="_blank"><img src="https://github.com/net2devcrypto/misc/blob/main/ytlogo2.png" width="150" height="40"></a>

#How to

Populate the "gateway" array with the gateway urls. Save and run.

The return from "checkLatency" would be the fastest url to make the API call.

#Code

```shell
const gateways = [
    {url:'http://eur.mywebbackend.io:8081/ipfs/'},
    {url:'http://usa.mywebbackend.io:8081/ipfs/'},
]

const apiGet = async (url) => {
  const options = {
    method: "GET",
    headers: {
      "Content-Type": "application/json"
    }
  }
  try {
    const response = await fetch(url, options);
    return response.body;
  } catch (error) {
    console.log(error);
  }
}

export async function checkLatency() {
    let times = [];
    let sites = [];
    for (let i = 0; i < gateways.length; i++) {
        let url = gateways[i].url + cid;
        let start = performance.now();
        const output = await apiGet(url);
        if (output != undefined){
            let end = performance.now();
            let latency = (end - start)
            console.log('url: ' + gateways[i].url + ' latency: ' + latency + 'ms')
            times.push(latency);
            sites.push(gateways[i].url);
        }
    }
    let lowest = Math.min(...times);
    let site = sites[times.indexOf(lowest)];
    console.log('Fastest API Gateway: ' + ' ' + site)
    return site;
}
```

Navigate to your web frontend and test!
