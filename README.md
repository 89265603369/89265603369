from highrise import 
BaseBot,
ChatEvent,
Highrise,
main,
UserJoinedEvent,
UserLeftEvent,
)
from highrise.models import (
AnchorPosition,
ChannelEvent,
ChannelRequest,
ChatEvent,
ChatRequest,
CurrencyItem,
EmoteEvent,
EmoteRequest,
Error,
FloorHitRequest,
GetRoomUsersRequest,
GetWalletRequest,
IndicatorRequest,
Item,
Position,
Reaction,
ReactionEvent,
ReactionRequest,
SessionMetadata,
TeleportRequest,
TipReactionEvent,
User,
UserJoinedEvent,
UserLeftEvent,
)
from asyncio import run as arun
import random

from webserver import keep_alive

class Bot(BaseBot):

async def on_start(self, SessionMetadata: SessionMetadata) -> None:
await self.highrise.walk_to(Position(0.5, 0, 5))
await self.highrise.chat("Hey I'm Back! Sorry For Inconvenience")

async def on_user_join(self, user: User) -> None:
print(f"@{user.username} Joined the room")
await self.highrise.chat(
f"\nWelcome @{user.username}! Blessed to have you here!\n")

async def on_tip(self, sender: User, receiver: User,
tip: CurrencyItem | Item) -> None:
print(
f"{sender.username} tipped {receiver.username} an amount of {tip.amount}"
)
goldAmount = tip.amount
emoteId = 'idle-dance-casual'
if goldAmount >= 5 and receiver.username == "iSeaweed":
await self.highrise.chat(
f"\nThank You {sender.username}!\nI'm so glad you tipped {tip.amount} to the host, keep showing love 💙"
)
await self.highrise.send_emote(emoteId, sender.id)
else:
await self.highrise.chat(
f"@{sender.username} tipped {tip.amount}G to the lovely @{receiver.username}"
)

async def run(self, room_id, token):
await main.main(self, room_id, token)

keep_alive()
if __name__ == "__main__":
room_id = "6454da2e30bc9ce44d29e882"
token = "55bfb3ae9d4389fd6d75ae4ce3c17a369cd28cde234c02d8b072a05d1605d58f"
arun(Bot().run(room_id, token))
