# RC522 RFID Reader

I am referring to the complete module, not just the chip.

**If you have metal behind the reader OR the tag, it won't be able to read it.**

To mitigate, get a piece of dielectric material, like ferrite and put that between the tag and metal, that is actually how the commercial solution is made.

The better solution is to just buy the metal tags, they are more expensive, but they work better with metal.

Just know this before starting your project.

It fking SUCKS, but is the cheapest, you can get it as low as 50 rupees.

Actually, MFRC522 is the name of the chip, but also commonly refers to the cheap board it is commonly found on.

Somehow the IRQ pin is not supported by the most popular library for reading rfid tags using it for arduino, idk how, that is just how it is.

It alternates between having a tag and not having a tag when sending the command to read a tag, Ping multiple times to get the actual result.

If the boot mode of NodeMCU is wrong, this fker is probably the culprit.

Also, buy in bulk, these things are fragile AF, and can randomly stop working if voltage is not to their liking. Could be dirty DC, could be slightly more voltage, could be slightly less voltage.

These don't have any VRMs or filtering caps, so make sure power is good.

Sometimes you may need to do a SPI Reset, in my experience, even when i had soldered the connections, at this point, i can't figure out why, may be the mfrc522 chip crashes for some reason. Just do a SPI reset regularly.
