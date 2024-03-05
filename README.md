# GTPDoor Network Detection

Repository for some basic detection for the [GTPDoor](https://doubleagent.net/telecommunications/backdoor/gtp/2024/02/27/GTPDOOR-COVERT-TELCO-BACKDOOR) implant as described by [haxrob](doubleagent.net).

As described the implant in it's default state (without being re-keyed) will use a hardcoded key of `135798642`, this leads to a basic detection method where we can use the `GTP ECHO Request` (type 1) to monitor for any packets containing this request, as well as any kind of responses that are of the `GTP ECHO Response` (type 2). If we use the [xbits](https://docs.suricata.io/en/latest/rules/xbits.html#xbits-keyword) feature of Suricata we can actually monitor for the presence of this implant within a network environment.

An example PCAP has been added which contains a simple trigger packet that was made by setting up the implant on a machine and crafting the trigger packet using Python.

The rules can be found in `gtpdoor.rules`, note that these rules will be written for usage with Suricata and are **not** Snort compatible.

An extra rule was added based on the [observation](https://doubleagent.net/telecommunications/backdoor/gtp/2024/02/27/GTPDOOR-COVERT-TELCO-BACKDOOR#gtp-firewall) of haxrob where invalid `GTP` protocol types are still handled by the implant, this detection is simply checking if the response packet is using an invalid `GTP` protocol type as the implant seems to just copy the `GTP` header values supplied by the client for the flags.
