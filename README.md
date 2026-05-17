# Fake Gift
> [!IMPORTANT]
> EDUCATIONAL PURPOSES ONLY!

> [!Note]
> See an example here: https://www.youtube.com/watch?v=5u5gc263Etw

## Function

### Minified (uncodumented)
```js
function gift(e,a){return!!giftable.includes(a)&&("super"==a?api.matchmakePlayer(e,"classic","../../super-rank-welcome"):api.matchmakePlayer(e,"classic",`../../pack-welcome/${a}`),api.matchmakePlayer(e,"classic_survival","permalocked2"),!0)}giftable=["super","y2k","spring","astro","bee","medieval","highSociety"];
```

### De-minified (and documented)
```js
/**
* Sends a player to a fake gifted screen for either super or a cosmetic.
* 
* @param playerId - the id of the player
* @param type - What you want to gift. Accepts either a pack, like `astro`, or `super`
* @returns {void}
*/
giftable = ["super", "y2k", "spring", "astro", "bee", "medieval", "highSociety"];
function gift(playerId, type) {
    if (!giftable.includes(type)) { return false; }
    if (type == "super") {
        api.matchmakePlayer(playerId, "classic", "../../super-rank-welcome");
    } else {
        api.matchmakePlayer(playerId, "classic", `../../pack-welcome/${type}`);
    }

    api.matchmakePlayer(playerId, "classic_survival", "permalocked2");
    return true;
}
```

## Command
### Paste into World Code along with the above function

### Commands
**gift** - "Gifts" a player a fake giftable. Either super or a cosmetic pack.
> *Syntax: `/gift (username | "me") type`*
- An example would be `/gift Tom astro`, which would "gift" Tom the Astro pack.
- Another example would be `/gift me high society`, which would "gift" you the High Society pack.

**giftTypes** - Lists out the gift types you can gift.
> *Syntax: `/giftTypes`*

### Copy and Paste
> [!Note]
> Optionally, you can specify a username or db id in the `ALLOWED` array. If no users are in the array, anyone can use the command.
#### Minified
```js
ALLOWED=[];

function playerCommand(e,t){let i=t.split(" ");if("giftTypes"==t)return api.sendMessage(e,[{str:`The format is \`/gift username giftName\`\n\nValid giftables include: ${giftable.join(", ")}`,style:{color:"#cef3ff"}}]),!0;if("gift"==i[0].toLowerCase()){if(ALLOWED.length>=1&&!ALLOWED.includes(api.getPlayerDbId(e))&&!ALLOWED.includes(api.getEntityName(e)))return!1;if(i.length<3)return api.sendMessage(e,[{str:`The format is \`/gift username giftName\`\n\nValid giftables include: ${giftable.join(", ")}`,style:{color:"#cef3ff"}}]),!0;let n;if("me"==i[1])n=e;else try{n=api.getPlayerId(i[1])}catch{return api.sendMessage(e,[{str:`${i[1]} is not a valid username.`,style:{color:"#cef3ff"}}]),!0}let l=format(joinAfter(t.split(" "),1));return api.log(`Got ${l}`),gift(n,l)?api.sendMessage(e,[{str:`Gifted ${i[2]} to ${i[1]}.`,style:{color:"#cef3ff"}}]):api.sendMessage(e,[{str:`${i[2]} is not a valid giftable.\n\nValid giftables include: ${giftable.join(", ")}`,style:{color:"#cef3ff"}}]),!0}}function joinAfter(e=[],t=0){return e.splice(t+1,e.length-1).join(" ")}function format(e=""){let t=e;if(t=t.replace(" ",""),t=t.toLowerCase(),giftable.includes(t))return t;for(let e in t){let i=t;if(i=i.split(""),i[e]=i[e].toUpperCase(),i=i.join(""),giftable.includes(i))return i}return!1}
```
---
#### De-minified
```js
function playerCommand(myId, txt) {
    let split = txt.split(" ");

    if (txt == "giftTypes") {
        api.sendMessage(myId, [
            { str: `The format is \`/gift username giftName\`\n\nValid giftables include: ${giftable.join(", ")}`, style: { color: "#cef3ff" } }
        ]);
        return true;
    }
    if (split[0].toLowerCase() == "gift") {
        if (ALLOWED.length >= 1) {
            if ((!ALLOWED.includes(api.getPlayerDbId(myId))) && (!ALLOWED.includes(api.getEntityName(myId)))) {
                return false;
            }
        }
        if (split.length < 3) {
            api.sendMessage(myId, [
                { str: `The format is \`/gift username giftName\`\n\nValid giftables include: ${giftable.join(", ")}`, style: { color: "#cef3ff" } }
            ]);
            return true;
        }

        let id;
        if (split[1] == "me") {
            id = myId;
        } else {
            try {
                id = api.getPlayerId(split[1]);
            } catch {
                api.sendMessage(myId, [
                    { str: `${split[1]} is not a valid username.`, style: { color: "#cef3ff" } }
                ]);
                return true;
            }
        }

        let giftInput = joinAfter(txt.split(" "), 1);
        let giftType = format(giftInput);
        api.log(`Got ${giftType}`);

        let status = gift(id, giftType);
        if (status) {
            api.sendMessage(myId, [
                { str: `Gifted ${split[2]} to ${split[1]}.`, style: { color: "#cef3ff" } }
            ]);
        } else {
            api.sendMessage(myId, [
                { str: `${split[2]} is not a valid giftable.\n\nValid giftables include: ${giftable.join(", ")}`, style: { color: "#cef3ff" } }
            ]);
        }
        return true;
    }
}

function joinAfter(array = [], idx = 0) {
    let newArray = array.splice(idx + 1, array.length - 1).join(" ");
    return newArray;
}

function format(txt = "") {
    let text = txt;
    text = text.replace(" ", "");
    text = text.toLowerCase();

    if (giftable.includes(text)) { return text ;}
    for (let t in text) {
        let caseProbe = text;

        caseProbe = caseProbe.split("");
        caseProbe[t] = caseProbe[t].toUpperCase();
        caseProbe = caseProbe.join("");

        if (giftable.includes(caseProbe)) {
            return caseProbe;
        }
    }
    return false;
}
```
