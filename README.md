## Building a OB Component
Building a Maint OB (OhBoy) is demonstrated as follows.

```py
# main.py
from maint import Client
from component import MyComponent # your component!

client = Client()

@client.event('message')
async def message(ctx):
  if ctx.author.bot:
    return # filter out bots
    
  await ctx.send(
    ob=[
      MyComponent(id="hello-world")
    ]
  )

@client.ohboy_autocomplete('hello-world')
async def my_component_handler(ctx, value):
  return [
    # note: awaits don't do anything here, but returns a valid action type
    await ctx.validate(["some good option you can choose", "...or this one"]),
    await ctx.reply("Hello, I'm very very smart 😈")
  ]
  

client.run()
```

```py
# component.py
from maint.ob import OBComponent, OB, Param

def MyComponent(id: str = "my-ob-component") -> OBComponent:
  return OBComponent([
    OB.Input(
      type="text",
      autocomplete=True
    ),
    parameters=[
      Param(type="input.value")
    ],
    id=id
  ])
```
