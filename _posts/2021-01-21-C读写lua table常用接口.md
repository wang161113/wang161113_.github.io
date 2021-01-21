---
layout:     post
title:      C/C++读写lua table常用接口
subtitle:   
date:       2021-01-21
author:     Gale_Studio
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - lua

---

# C/C++读写lua table常用接口

```c
//lua_gettable
lua_getglobal(L, "mytable")  //<== push mytable
lua_pushnumber(L, 1)        //<== push key 1
lua_gettable(L, -2)         //<== pop key 1, push mytable[1]

//lua_settable
lua_getglobal(L, "mytable") //<== push mytable
lua_pushnumber(L, 1)        //<== push key 1
lua_pushstring(L, "abc")    //<== push value "abc"
lua_settable(L, -3)         //<== mytable[1] = "abc", pop key & value
```

```c
//lua_rawget:
//用法同lua_gettable,但更快(因为当key不存在时不用访问元方法__index)

//lua_rawset:
//用法同lua_settable,但更快(因为当key不存在时不用访问元方法__newindex)
```

```c
//lua_rawgeti必须为数值键
lua_getglobal(L, "mytable") //<== push mytable
lua_rawgeti(L, -1, 1)      // <== push mytable[1]，作用同下面两行调用
lua_pushnumber(L, 1)      //<== push key 1
lua_rawget(L,-2)        //  <== pop key 1, push mytable[1]

//lua_rawseti必须为数值键
lua_getglobal(L, "mytable") //<== push mytable
lua_pushstring(L, "abc")   // <== push value "abc"
lua_rawseti(L, -2, 1)       //<== mytable[1] = "abc", pop value "abc"
```

```c
//lua_getfield必须为字符串键
lua_getglobal(L, "mytable") //<== push mytable
lua_getfield(L, -1, "x")    //<== push mytable["x"]，作用同下面两行调用
lua_pushstring(L, "x")    //<== push key "x"
lua_gettable(L,-2)        //<== pop key "x", push mytable["x"]

//lua_setfield必须为字符串键
lua_getglobal(L, "mytable") //<== push mytable
lua_pushstring(L, "abc")    //<== push value "abc"
lua_setfield(L, -2, "x")    //<== mytable["x"] = "abc", pop value "abc"
```