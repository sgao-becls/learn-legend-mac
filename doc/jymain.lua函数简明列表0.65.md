# jymain.lua 函数列表

以下按原 `jymain.lua函数简明列表0.65.txt` 整理为 Markdown，每行对应一个函数及其简要说明。

## 核心与流程

| 函数 | 说明 |
|------|------|
| `IncludeFile()` | 导入其他模块 |
| `SetGlobal()` | 设置游戏内部使用的全程变量 |
| `JY_Main()` | 主程序入口 |
| `myErrFun(err)` | 错误处理，打印错误信息 |
| `JY_Main_sub()` | 真正的游戏主程序入口 |
| `CleanMemory()` | 清理 Lua 内存 |
| `NewGame()` | 选择新游戏，设置主角初始属性 |
| `Game_Cycle()` | 游戏主循环 |

## 地图与场景

| 函数 | 说明 |
|------|------|
| `Init_MMap()` | 初始化主地图数据 |
| `Game_MMap()` | 主地图 |
| `Init_SMap(showname)` | 初始化场景数据 |
| `Cal_PicClip(dx1, dy1, pic1, id1, dx2, dy2, pic2, id2)` | 计算贴图改变形成的 Clip 裁剪 |
| `MergeRect(r1, r2)` | 合并矩形 |
| `ClipRect(r)` | 对矩形进行屏幕剪裁 |
| `GetMyPic()` | 计算主角当前贴图 |
| `AddMyCurrentPic()` | 增加当前主角走路动画帧 |
| `CanEnterScene(x, y)` | 场景是否可进 |
| `Cal_EnterSceneXY()` | 计算哪些坐标可以进入场景 |
| `Game_SMap()` | 场景处理主函数 |
| `SceneCanPass(x, y)` | 场景坐标 `(x,y)` 是否可以通过 |
| `DtoSMap()` | D* 中的事件数据复制到 S* 中，同时处理动画效果 |
| `DrawSMap(fastdraw)` | 绘场景地图 |
| `ChangeMMap(x, y, direct)` | 改变大地图坐标 |
| `ChangeSMap(sceneid, x, y, direct)` | 改变当前场景 |

## 菜单与队伍

| 函数 | 说明 |
|------|------|
| `MMenu()` | 主菜单 |
| `Menu_System()` | 系统子菜单 |
| `Menu_Exit()` | 离开菜单 |
| `Menu_SaveRecord()` | 保存进度菜单 |
| `Menu_ReadRecord()` | 读取进度菜单 |
| `Menu_Status()` | 状态子菜单 |
| `Menu_PersonExit()` | 离队 Exit |
| `SelectTeamMenu(x, y)` | 队伍选择人物菜单 |
| `GetTeamNum()` | 得到队友个数 |
| `ShowPersonStatus(teamid)` | 显示队友状态 |
| `ShowPersonStatus_sub(id, page)` | 显示人物状态页面 |
| `TrainNeedExp(id)` | 计算人物修炼物品成功需要的点数 |
| `Menu_Doctor()` | 医疗菜单 |
| `ExecDoctor(id1, id2)` | 执行医疗 |
| `Menu_DecPoison()` | 解毒 |
| `ExecDecPoison(id1, id2)` | 执行解毒 |
| `Menu_Thing()` | 物品菜单 |
| `SelectThing(thing, thingnum)` | 显示物品菜单 |

## 存档与数据

| 函数 | 说明 |
|------|------|
| `LoadRecord(id)` | 读取游戏进度 |
| `SaveRecord(id)` | 写游戏进度 |
| `filelength(filename)` | 得到文件长度 |
| `GetS(id, x, y, level)` | 读 S* 数据 |
| `SetS(id, x, y, level, v)` | 写 S* |
| `GetD(Sceneid, id, i)` | 读 D* |
| `SetD(Sceneid, id, i, v)` | 写 D* |
| `GetDataFromStruct(data, offset, t_struct, key)` | 从结构中取数据 |
| `SetDataFromStruct(data, offset, t_struct, key, v)` | 向结构中存数据 |
| `LoadData(t, t_struct, data)` | 从二进制串读到表 `t` |
| `SaveData(t, t_struct, data)` | 数据写入 `data` Byte 数组 |

## 绘图与输入工具

| 函数 | 说明 |
|------|------|
| `limitX(x, minv, maxv)` | 限制 x 的范围 |
| `RGB(r, g, b)` | 设置颜色 RGB |
| `GetRGB(color)` | 分离颜色的 RGB 分量 |
| `WaitKey()` | 等待键盘输入 |
| `DrawBox(x1, y1, x2, y2, color)` | 绘制带背景的白色方框 |
| `DrawBox_1(x1, y1, x2, y2, color)` | 绘制四角凹进的方框 |
| `DrawString(x, y, str, color, size)` | 显示阴影字符串 |
| `DrawStrBox(x, y, str, color, size)` | 显示带框的字符串 |
| `DrawStrBoxYesNo(x, y, str, color, size)` | 显示字符串并询问 Y/N |
| `DrawStrBoxWaitKey(s, color, size)` | 显示字符串并等待击键 |
| `Rnd(i)` | 随机数 |
| `AddPersonAttrib(id, str, value)` | 增加人物属性 |
| `PlayMIDI(id)` | 播放 midi |
| `PlayWavAtk(id)` | 播放音效 atk*** |
| `PlayWavE(id)` | 播放音效 e** |
| `ShowScreen(flag)` | 刷新屏幕显示 |
| `ShowMenu(menuItem, numItem, numShow, x1, y1, x2, y2, isBox, isEsc, size, color, selectColor)` | 通用菜单函数 |
| `ShowMenu2(menuItem, numItem, numShow, x1, y1, x2, y2, isBox, isEsc, size, color, selectColor)` | 通用菜单函数 |

## 物品

| 函数 | 说明 |
|------|------|
| `UseThing(id)` | 物品使用 |
| `DefaultUseThing(id)` | 缺省物品使用函数 |
| `UseThing_Type0(id)` | 剧情物品使用 |
| `UseThing_Type1(id)` | 装备物品使用 |
| `CanUseThing(id, personid)` | 判断是否可以装备或修炼物品 |
| `UseThing_Type2(id)` | 秘籍物品使用 |
| `UseThing_Type3(id)` | 药品物品使用 |
| `UseThingEffect(id, personid)` | 药品使用实际效果 |
| `UseThing_Type4(id)` | 暗器物品使用 |

## 事件与对话

| 函数 | 说明 |
|------|------|
| `EventExecute(id, flag)` | 事件调用主入口 |
| `oldEventExecute(flag)` | 调用原有指定位置的函数 |
| `oldCallEvent(eventnum)` | 执行旧的事件函数 |
| `GenTalkString(str, n)` | 产生对话显示需要的字符串 |
| `Talk(s, personid)` | 最简单版本对话 |
| `TalkEx(s, headid, flag)` | 复杂版本对话 |
| `instruct_test(s)` | （测试） |
| `instruct_0()` | 清屏 |
| `instruct_1(talkid, headid, flag)` | 对话 |
| `GenTalkIdx()` | 生成对话索引文件 |
| `ReadTalk(talkid)` | 从文件读取一条对话 |
| `instruct_2(thingid, num)` | 得到物品 |
| `instruct_2_sub()` | 声望>200 以及 14 天书后得到武林帖 |
| `instruct_3(sceneid, id, v0, …, v10)` | 修改 D* |
| `instruct_4(thingid)` | 是否使用物品触发 |
| `instruct_5()` | 选择战斗 |
| `instruct_6(warid, tmp, tmp, flag)` | 战斗 |
| `instruct_7()` | 已翻译为 return |
| `instruct_8(musicid)` | 改变主地图音乐 |
| `instruct_9()` | 是否要求加入队伍 |
| `instruct_10(personid)` | 加入队员 |
| `instruct_11()` | 是否住宿 |
| `instruct_12()` | 住宿，回复体力 |
| `instruct_13()` | 场景变亮 |
| `instruct_14()` | 场景变黑 |
| `instruct_15()` | game over |
| `instruct_16(personid)` | 队伍中是否有某人 |
| `instruct_17(sceneid, level, x, y, v)` | 修改场景图形 |
| `instruct_18(thingid)` | 是否有某种物品 |
| `instruct_19(x, y)` | 改变主角位置 |
| `instruct_20()` | 判断队伍是否满 |
| `instruct_21(personid)` | 离队 |
| `instruct_22()` | 内力降为 0 |
| `instruct_23(personid, value)` | 设置用毒 |
| `instruct_24()` | |
| `instruct_25(x1, y1, x2, y2)` | 场景移动 |
| `instruct_26(sceneid, id, v1, v2, v3)` | 增加 D* 编号 |
| `instruct_27(id, startpic, endpic)` | 显示动画 |
| `instruct_28(personid, vmin, vmax)` | 判断品德 |
| `instruct_29(personid, vmin, vmax)` | 判断攻击力 |
| `instruct_30(x1, y1, x2, y2)` | 主角走动 |
| `instruct_30_sub(direct)` | 主角走动 sub |
| `instruct_31(num)` | 判断是否够钱 |
| `instruct_32(thingid, num)` | 增加物品 |
| `instruct_33(personid, wugongid, flag)` | 学会武功 |
| `instruct_34(id, value)` | 资质增加 |
| `instruct_35(personid, id, wugongid, wugonglevel)` | 设置武功 |
| `instruct_36(sex)` | 判断主角性别 |
| `instruct_37(v)` | 增加品德 |
| `instruct_38(sceneid, level, oldpic, newpic)` | 修改场景某层贴图 |
| `instruct_39(sceneid)` | 打开场景 |
| `instruct_40(v)` | 改变主角方向 |
| `instruct_41(personid, thingid, num)` | 其他人员增加物品 |
| `instruct_42()` | 队伍中是否有女性 |
| `instruct_43(thingid)` | 是否有某种物品 |
| `instruct_44(id1, startpic1, endpic1, id2, startpic2, endpic2)` | 同时显示两个动画 |
| `instruct_45(id, value)` | 增加轻功 |
| `instruct_46(id, value)` | 增加内力 |
| `instruct_47(id, value)` | |
| `instruct_48(id, value)` | 增加生命 |
| `instruct_49(personid, value)` | 设置内力属性 |
| `instruct_50(id1, id2, id3, id4, id5)` | 判断是否有 5 种物品 |
| `instruct_51()` | 问软体娃娃 |
| `instruct_52()` | 看品德 |
| `instruct_53()` | 看声望 |
| `instruct_54()` | 开放其他场景 |
| `instruct_55(id, num)` | 判断 D* 编号的触发事件 |
| `instruct_56(v)` | 增加声望 |
| `instruct_57()` | 高昌迷宫劈门 |
| `instruct_58()` | 武道大会比武 |
| `instruct_59()` | 全体队员离队 |
| `instruct_60(sceneid, id, num)` | 判断 D* 图片 |
| `instruct_61()` | 判断是否放完 14 天书 |
| `instruct_62(id1, startnum1, endnum1, id2, startnum2, endnum2)` | 播放时空机动画，结束 |
| `instruct_63(personid, sex)` | 设置性别 |
| `instruct_64()` | 小宝卖东西 |
| `instruct_65()` | 小宝去其他客栈 |
| `instruct_66(id)` | 播放音乐 |
| `instruct_67(id)` | 播放音效 |

## 战斗：全局与地图

| 函数 | 说明 |
|------|------|
| `WarSetGlobal()` | 设置战斗全程变量 |
| `WarLoad(warid)` | 加载战斗数据 |
| `WarMain(warid, isexp)` | 战斗主函数 |
| `War_PersonLostLife()` | 每回合中毒或受伤掉血 |
| `War_EndPersonData(isexp, warStatus)` | 战斗后设置人物参数 |
| `War_AddPersonLevel(pid)` | 人物是否升级 |
| `War_PersonTrainBook(pid)` | 战斗后修炼秘籍是否成功 |
| `War_PersonTrainDrug(pid)` | 战斗后是否修炼出物品 |
| `War_isEnd()` | 战斗是否结束 |
| `WarSelectTeam()` | 选择我方参战人 |
| `WarSelectMenu(newmenu, newid)` | 选择战斗人菜单 |
| `WarSelectEnemy()` | 选择敌方参战人 |
| `WarLoadMap(mapid)` | 加载战斗地图 |
| `GetWarMap(x, y, level)` | 取战斗地图数据 |
| `SetWarMap(x, y, level, v)` | 存战斗地图数据 |
| `CleanWarMap(level, v)` | |
| `WarDrawMap(flag, v1, v2, v3)` | |
| `WarPersonSort()` | 战斗人物按轻功排序 |
| `WarSetPerson()` | 设置战斗人物位置 |
| `WarCalPersonPic(id)` | 计算战斗人物贴图 |

## 战斗：手动与移动

| 函数 | 说明 |
|------|------|
| `War_Manual()` | 手动战斗 |
| `War_Manual_Sub()` | 手动战斗菜单 |
| `War_GetMinNeiLi(pid)` | 所有武功中需要内力最少的 |
| `WarShowHead()` | 显示战斗人头像 |
| `War_MoveMenu()` | 执行移动菜单 |
| `War_CalMoveStep(id, stepmax, flag)` | 计算可移动步数 |
| `War_FindNextStep(steparray, step, flag)` | 设置下一步可移动坐标 |
| `War_CanMoveXY(x, y, flag)` | 坐标是否可通过（移动判断） |
| `War_SelectMove()` | 选择移动位置 |
| `War_MovePerson(x, y)` | 移动人物到 `(x,y)` |

## 战斗：攻击与效果

| 函数 | 说明 |
|------|------|
| `War_FightMenu()` | 执行攻击菜单 |
| `War_Fight_Sub(id, wugongnum, x, y)` | 执行战斗 |
| `War_FightSelectType0(wugong, level, x1, y1)` | 点攻击 |
| `War_FightSelectType1(wugong, level, x, y)` | 线攻击 |
| `War_FightSelectType2(wugong, level)` | 十字攻击 |
| `War_FightSelectType3(wugong, level, x1, y1)` | 面攻击 |
| `War_Direct(x1, y1, x2, y2)` | 计算人方向 |
| `War_ShowFight(pid, wugong, wugongtype, level, x, y, eft)` | 显示战斗动画 |
| `War_WugongHurtLife(emenyid, wugong, level)` | 武功伤害生命 |
| `War_WugongHurtNeili(enemyid, wugong, level)` | 武功伤害内力 |
| `War_PoisonMenu()` | 用毒菜单 |
| `War_PoisonHurt(pid, emenyid)` | 计算敌人中毒点数 |
| `War_DecPoisonMenu()` | 解毒菜单 |
| `War_DoctorMenu()` | 医疗菜单 |
| `War_ExecuteMenu(flag, thingid)` | 执行医疗、解毒、用毒、暗器 |
| `War_ExecuteMenu_Sub(x1, y1, flag, thingid)` | 上述子函数（自动医疗也可调用） |
| `War_ThingMenu()` | 战斗物品菜单 |
| `War_UseAnqi(id)` | 战斗使用暗器 |
| `War_AnqiHurt(pid, emenyid, thingid)` | 计算暗器伤害 |
| `War_RestMenu()` | 休息 |
| `War_WaitMenu()` | 等待，当前战斗人调到队尾 |
| `War_StatusMenu()` | 战斗中显示状态 |
| `War_AutoMenu()` | 设置自动战斗 |

## 战斗：自动

| 函数 | 说明 |
|------|------|
| `War_Auto()` | 自动战斗主函数 |
| `War_Think()` | 思考如何战斗 |
| `War_ThinkDrug(flag)` | 能否吃药加参数 |
| `War_ThinkDoctor()` | 是否给自己医疗 |
| `War_AutoFight()` | 执行自动战斗 |
| `War_AutoSelectWugong()` | 自动选合适武功 |
| `War_AutoSelectEnemy()` | 选择战斗对手 |
| `War_AutoSelectEnemy_near()` | 选择最近对手 |
| `War_AutoMove(wugongnum)` | 自动往敌人方向移动 |
| `War_GetCanFightEnemyXY(scope)` | 可走到并攻击敌人的最近位置 |
| `War_AutoCalMaxEnemyMap(wugongid, level)` | 地图上每格可攻击敌人数 |
| `War_AutoCalMaxEnemy(x, y, wugongid, level)` | 从 `(x,y)` 最多击中几个敌人 |
| `War_AutoExecuteFight(wugongnum)` | 自动执行战斗并显示攻击动画 |
| `War_AutoEscape()` | 逃跑 |
| `War_AutoEatDrug(flag)` | 吃药加参数 |
| `War_AutoDoctor()` | 自动医疗 |

## 其他

| 函数 | 说明 |
|------|------|
| `Cls(x1, y1, x2, y2)` | 清除屏幕 |
