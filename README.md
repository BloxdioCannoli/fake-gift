# Fake Gift
> [!IMPORTANT]
> EDUCATIONAL PURPOSES ONLY!

## Function
```js
/**
* Sends a player to a fake gifted screen for either super or a cosmetic.
* 
* @param playerId - the id of the player
* @param type - What you want to gift. Accepts either a pack, like `astro`, or `super`
* @returns {void}
*/
giftable = ["super", "astro", "y2k", "spring", "bee", "medieval"];
function gift(playerId, type) {
    if (!giftable.includes(type)) {return false}
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
### Paste into World Code.
> Syntax: `/gift username type`
```js
giftable = ["super", "astro", "y2k", "spring", "bee", "medieval"];
function gift(playerId, type) {
    if (!giftable.includes(type)) {return false}
    if (type == "super") {
        api.matchmakePlayer(playerId, "classic", "../../super-rank-welcome");
    } else {
        api.matchmakePlayer(playerId, "classic", `../../pack-welcome/${type}`);
    }

    api.matchmakePlayer(playerId, "classic_survival", "permalocked2");
    return true;
}

function playerCommand(myId, txt) {
    let split = txt.split(" ");
    if (split[0].toLowerCase() == "gift") {
        let id;
        try {
            id = api.getPlayerId(split[1]);
        } catch {
            api.sendMessage(myId, [
                { str: `${split[1]} is not a valid username.`, style: { color: "#cef3ff" } }
            ]);
            return true;
        }

        let status = gift(id, split[2]);
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
```
