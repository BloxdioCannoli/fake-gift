# Fake Gift Info
> [!IMPORTANT]
> EDUCATIONAL PURPOSES ONLY!

## How it works
We can make use of `../` in `api.matchmakePlayer` to go to (mostly) any url in Bloxd.io. Using this, we can do to a purchase confirmation URL such as `https://bloxd.io/super-rank-welcome` or `https://bloxd.io/pack-welcome/:pack` (eg `https://bloxd.io/pack-welcome/astro`). However, `api.matchmakePlayer(playerId, "../../[purchase URL]")` on its own does not do what we want it to. Instead, it hides the UI and requires the player to reload their page in order to see it. We fix this by reloading *as soon as possible.* So GlitchHunter, FatalErrors and I did some testing, and crashing the game doesn't work. Neither does kicking the player. However, I discovered I could matchmake the player a second time into a full lobby, and then realized this also works for *locked* lobbies.

Thus, we matchmake the player *twice*. First onto the purchase welcome screen, and then immediately onto a locked lobby. This refreshes the player's page and doesn't require any setup -- just pasting something into World Code and running a command.

As for the command, it has some basic text parsing to support phrases such as `High Society`, `highSocity`, and `h ighsociety`, and also runs some checks that both the giftable and the gifted player exist. Any errors are handled with a private message to the command-sender, so it's impossible to notice in chat.

If all checks pass, then the gifted player gets very briefly sent to a loading screen and then redirected to the confirmation screen. If they don't have the cosmetic or rank, it has an orange box that states "You should get {Super Rank | your cosmetics} instantly, but it could take up to an hour to apply. If you don't have {Super Rank | them} after an hour, contact help@bloxd.io.". The gifted player retains full control over mouse movement and pressing the back arrow takes them back to the lobby.

## Examples
- Demonstration Video: https://www.youtube.com/watch?v=5u5gc263Etw
- World (anyone can use `/gift`) - [fakegifting2](https://bloxd.io/play/classic/fakegifting2)
- World (only approved users can use `/gift`) - [fakegifting](https://bloxd.io/play/classic/fakegifting)
