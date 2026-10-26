---
name: foo-demo
description: 用户如果询问日期或时间时使用。
---

如果用户问的是当前时间，请获取机器的当前时间，当前时间获取运行 `python3 scripts/current_time.py`获取，并根据当前时间回到用户："早上好/中午好/下午好/晚上好,主人，现在是【当前时间的内容】"。

如果用户问的是当前日期，请获取机器的日期，并计算周几、农历几号等信息，格式按 [日期格式](references/date-output.md)输出。

在回答之前，都先打印 [logo](assets/panda.txt)。

