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
**gift** - Gifts a player a fake giftable. Either super or a cosmetic pack.
> *Syntax: `/gift (username | "me") type`*
- An example would be `/gift Tom astro`, which would gift Tom the Astro pack.
- A bad example would be `/gift Tom Astro`, which would **not** gift Tom the Astro pack.

**giftTypes** - Lists out the gift types you can gift.
> *Syntax: `/giftTypes`*
### Code
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
        if (split.length != 3) {
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
