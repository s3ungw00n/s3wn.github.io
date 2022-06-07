---
layout  : post
title   : FastAPI - RequestBody
date    : 2022-06-08 01:45:00 +0900
updated : 2022-06-08 01:45:00 +0900
tags    : fastapi
toc     : true
public  : true
parent  : 
latex   : false
---

# FastAPI 

## RequestBody
  pydandic Model을 사용하거나, FastAPI.Body() 기본값을 사용하여 request body를
받을 수 있다. 
```
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```

## QueryParameter validation
  FastAPI.Query()를 parameter default value로 넣어주면 해당 query에 대한 validation을 할 수 있다.
  
```
@app.get("/items/")
def read_items(q: str | None = Query(default=None, max_length=5, regex="^\d{0,5}$")):
    return q
```
  
