# solid-doodle
Mee
 " Made by @e3ris for Ultroid "
# < https://github.com/TeamUltroid/Ultroid >

"""âœ˜ **Send any Installed Plugin to the Chat!**

>> Works for [addons/plugins] folder!
>> Now Pastes the file too.
>> Use : `{i}send <plugin_name>`
"""

import os

from . import *


@ultroid_cmd(pattern="send ?(.*)")
async def semd_plugin(ult):
    if ult.fwd_from:
        return
    repos = "https://github.com/TeamUltroid/Ultroid"
    args = ult.pattern_match.group(1)
    if not args:
        return await eod(ult, "`Give a plugin name to send.`")
    eris = await eor(ult, "`...`")
    if args.endswith(".py"):
        args = args[:-3]    
    plugins_ = (f"plugins/{args}.py", f"addons/{args}.py")
    for plug in plugins_:
        if os.path.exists(plug):
            try:
                linky = await pastor(plug)
                p_a = "Plugin" if plug.startswith("plugins") else "Add-on"
                cap_ = f"<b>>> <a href='{linky}'>Pasted Here!</a></b> \n" if linky else ''
                cap = (
                    f"<b>>> {p_a} :</b><code> {args}</code> \n{cap_} \n" \
                    f"Â© <a href='{repos}'>Team Ultroid</a>"
                )
                await ult.client.send_file(
                    ult.chat_id,
                    plug,
                    caption=cap,
                    parse_mode="html",
                    thumb="resources/extras/ultroid.jpg",
                    reply_to=ult.reply_to_msg_id
                )
                await eris.delete()
            except Exception as fx:
                return await eod(eris, str(fx))
    else:
        return await eod(
            eris,
            f"No plugins were found for: `{args}`",
            time=10,
        )          


async def pastor(path):
    try:
        from addons.ipaste import rpaste
        filex = open(path, "r")
        data_ = filex.read()
        filex.close()
        link = rpaste(data_)
        if isinstance(link, dict):
            return link.get("link")
        else:
            return None
    except:
        return None
