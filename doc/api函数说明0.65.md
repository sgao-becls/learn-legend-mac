# 主程序给 Lua 提供的接口 API 说明

## 1. 主程序引擎架构

主程序用 SDL 实现了一个最基本的跨平台的图形引擎，Lua 可以调用这些函数进行游戏编程。

### 引擎特点

1. 使用 SDL 库，包括 SDL_image、SDL_mixer、SDL_ttf、smpeg。支持 16/24/32 色，支持 midi/mp3 等音乐格式，支持加载 bmp/png/jpg 等图片格式。
2. 窗口可自定义大小，但不得小于 320×240。
3. 支持加载 png 位图文件以及含有 png 文件的 idx/grp 贴图文件。
4. 显示贴图采用缓存结构，使用者无需关心如何加载。这样既保持一定效率，又减小对内存的使用。
5. 只采用一个显示表面，所有的图形操作都在这一个表面上进行。

## 2. Lua 脚本

目前使用 **Lua 5.1.2**。运行时主程序自动调用脚本 `script\jymain.lua`，并运行其中的 `JY_Main()` 函数。

游戏编写者从这里开始编写游戏函数。

## 3. API 函数列表

以下函数都是主程序引擎提供的、可在 Lua 中调用的函数。

> **注意：** 对这些 API 没有做更多的参数检查，因此要确保输入的参数是合理的，否则程序可能会出错，也可能什么都不做。

### 调试与时间

- **`lib.Debug(str)`**  
  在主程序目录下的 `debug.txt` 文件中输出调试字符串。

- **`lib.EnableKeyRepeat(delay, interval)`**（新增）  
  设置键盘重复率。`delay` 为第一个重复的延迟毫秒数，`interval` 为多少毫秒重复一次。ESC、RETURN 和 SPACE 键已取消重复，一直按下也只认为是按下一次。

- **`lib.GetKey()`**  
  得到当前按键键码，键码定义参见 SDL 文档。  
  此函数处理键盘缓冲区和键盘重复率，返回的是从上次调用以来曾经按下的键，并且只处理按下一个键的情况。因此如果需要清除键盘缓冲区，需要先调用一次此函数。

- **`lib.GetTime`**  
  返回开机到当前的毫秒数。

- **`lib.Delay(t)`**  
  延时 `t` 毫秒。

### 字符集与裁剪、绘图

- **`lib.CharSet(str, flag)`**（修改）  
  返回把 `str` 转换后的字符串。  
  - `flag = 0`：Big5 → GBK  
  - `= 1`：GBK → Big5  
  - `= 2`：Big5 → Unicode  
  - `= 3`：GBK → Unicode  

- **`lib.SetClip(x1, y1, x2, y2)`**  
  设置裁剪窗口。设置以后所有对表面的绘图操作都只影响 `(x1,y1)-(x2,y2)` 的矩形框内部。若 `x1,y1,x2,y2` 均为 0，则裁剪窗口为整个表面。  
  本函数在内部维护一个裁剪窗口列表，`ShowScreen` 使用此列表来更新实际的屏幕显示。每调用一次，裁剪窗口数量 +1，最多为 20 个。当 `x1,y1,x2,y2` 均为 0 时，清除全部裁剪窗口。

- **`lib.FillColor(x1, y1, x2, y2, color)`**  
  用颜色 `color` 填充表面的矩形 `(x1,y1)-(x2,y2)`，`color` 为 32 位 RGB（从最高字节依次为 0,R,G,B）。若 `x1,y1,x2,y2` 均为 0，则填充整个表面。

- **`lib.Background(x1, y1, x2, y2, Bright)`**  
  把表面内矩形 `(x1,y1)-(x2,y2)` 内所有点的亮度降低为 `Bright` 倍。`Bright` 取值为 0–256：0 表示全黑，256 表示亮度不变。

- **`lib.DrawRect(x1, y1, x2, y2, color)`**  
  绘制矩形 `(x1,y1)-(x2,y2)`，线框为单个像素，颜色为 `color`。

- **`lib.DrawStr(x, y, str, color, size, fontname, charset, OScharset)`**（修改）  
  在 `(x,y)` 位置写字符串 `str`。  
  - `color`：字体颜色  
  - `size`：字体像素大小  
  - `fontname`：字体名字  
  - `charset`：字符串字符集，0 GBK，1 BIG5  
  - `OScharset`：0 显示简体，1 显示繁体  
  此函数直接显示阴影字，在 Lua 中不用单独处理阴影字，可提高字符串显示速度。

### 表面显示

- **`lib.ShowSurface(flag)`**  
  把表面显示到屏幕。  
  - `flag = 0`：显示全部表面  
  - `= 1`：按照 `SetClip` 设置的矩形依次显示；如果没有矩形，则不显示  
  在调用此函数前，表面上的数据是不会显示的。

- **`lib.ShowSlow(t, flag)`**  
  把表面缓慢显示到屏幕。`t` 为亮度每变化一次的间隔毫秒数；为 16/32 位兼容，一共有 32 阶亮度变化。  
  - `flag`：0 从暗到亮，1 从亮到暗  

### 贴图

- **`lib.PicInit(str)`**  
  初始化贴图 Cache，`str` 为调色板文件。在转换场景前调用，清空所有保存的贴图文件信息。第一次调用时需要加载调色板，以后设置 `str` 为空字符串即可。

- **`lib.PicLoadFile(idxfilename, grpfilename, id)`**  
  加载贴图文件信息。`idxfilename`/`grpfilename` 为 idx/grp 文件名；`id` 为加载编号，0–39（最大 40 个）。若原来已有则覆盖。

- **`lib.PicLoadCache(id, picid, x, y, flag, value)`**  
  加载 `id` 所指示的贴图文件中编号为 **`picid/2`**（为保持兼容仍除 2）的贴图到表面的 `(x,y)`。`id` 为 `lib.PicLoadFile` 的加载编号。  
  **`flag` 各位含义（缺省均为 0）：**  
  - **B0**：0 考虑偏移 xoff、yoff；=1 不考虑偏移量（物品图像、人物头像等可设为 1）。  
  - **B1**：与背景 alpha 混合显示，`value` 为 alpha 值 (0–256)，0 表示透明。目前不支持 png 中单像素 alpha，只考虑透明与不透明。  
  - **B2**：=1 全黑（先全黑再 Alpha，仅当 B1 为 1 时有意义）。  
  - **B3**：=1 全白（同上）。  
  `value`：当 `flag` 设置 alpha 时为 alpha 值。当 `flag=0` 时，`flag` 和 `value` 可省略。  
  战斗中特殊效果可用 B1、B2、B3；Lua 无单独位或时可用加法，例如 B1+B2：`flag=2+4`；B1+B3：`flag=2+8`。

- **`lib.GetPicXY(id, picid)`**  
  得到贴图大小，返回贴图宽、高、x 偏移、y 偏移。

### 全屏与图片、音视频

- **`lib.FullScreen()`**  
  切换全屏和窗口，调用一次改变一次状态。

- **`lib.LoadPicture(filename, x, y)`**  
  显示图片文件到位置 `x,y`，支持 bmp/png/jpg 等。若 `x=-1, y=-1` 则显示在屏幕中间。会在内存中缓存上一次加载的图片以加快重复加载；用空文件名调用可清除占用的内存。

- **`lib.PlayMIDI(filename)`**  
  重复播放 MID 文件；`filename` 为空字符串则停止当前 midi。

- **`lib.PlayWAV(filename)`**  
  播放音效 AVI 文件 `filename`。

- **`lib.PlayMPEG(filename, key)`**  
  播放 mpeg1 视频；`key` 为停止播放按键的键码，一般设为 Esc。

### 主地图

- **`lib.LoadMMap(filename1, filename2, filename3, filename4, filename5, xmax, ymax, x, y)`**  
  加载主地图的 5 个结构文件 `*.002`。贴图文件依次为 earth、surface、building、buildx、buildy。`xmax, ymax` 为主地图宽高（目前均为 480），`x,y` 为主角坐标。

- **`lib.GetMMap(x, y, flag)`**  
  取主地图结构相应坐标的值：`flag=0` earth，1 surface，2 building，3 buildx，4 buildy。

- **`lib.DrawMMap(x, y, mypic)`**  
  在表面绘制主地图。`(x,y)` 为主角坐标；`mypic` 为主角贴图编号（**实际编号，不用除 2**）。

- **`lib.UnloadMMap()`**  
  释放主地图占用内存。

### 场景地图 S* / D*

- **`lib.LoadSMap(Sfilename, tempfilename, num, x_max, y_max, Dfilename, d_num1, d_num2)`**  
  加载场景地图数据 S* 和 D*。`Sfilename`：s* 文件名；`tempfilename`：临时 S*；`num`：场景个数；`x_max, y_max`：场景宽高；`Dfilename`：D* 文件名；`d_num1`：每个场景几个 D 数据（应为 200）；`d_num2`：每个 D 几个数据（应为 11）。文件名只需 `.grp`，因每个场景数据固定，不需要 `.idx`。

- **`lib.SaveSMap(Sfilename, Dfilename)`**  
  保存 S* 和 D*。

- **`lib.GetS(id, x, y, level)`** / **`lib.SetS(id, x, y, level, v)`**  
  读/写 S* 数据（`id` 场景编号，`x,y` 坐标，`level` 层数）。

- **`lib.GetD(Sceneid, id, i)`** / **`lib.SetD(Sceneid, id, i, v)`**  
  读/写 D* 数据。

- **`lib.DrawSMap(sceneid, x, y, xoff, yoff, mypic)`**  
  绘场景地图。`sceneid`：场景编号；`x,y`：主角坐标；`xoff,yoff`：场景中心偏移；`mypic`：主角贴图 **×2**。

### 战斗地图

- **`lib.LoadWarMap(WarIDXfilename, WarGRPfilename, mapid, num, x_max, y_max)`**  
  加载战斗地图。`mapid`：战斗地图编号；`num`：数据层数（应为 6）：  
  0 地面，1 建筑，2 战斗人战斗编号，3 可移动位置，4 命中效果，5 战斗人对应贴图。`x_max, y_max`：地图大小。战斗地图只读取两层数据，其余为工作数据区。

- **`lib.GetWarMap(x, y, level)`** / **`lib.SetWarMap(x, y, level, v)`**  
  取/存战斗地图数据。

- **`lib.CleanWarMap(level, v)`**  
  给 `level` 层战斗数据全部赋值 `v`。

- **`lib.DrawWarMap(flag, x, y, v1, v2)`**（原文另有 v3 说明）  
  绘战斗地图：  
  - `flag=0`：绘制基本战斗地图  
  - `=1`：显示可移动路径，`(v1,v2)` 当前移动坐标，白色背景（雪地战斗）  
  - `=2`：显示可移动路径，黑色背景  
  - `=3`：命中的人物用白色轮廓显示  
  - `=4`：战斗动作动画；`v1` 战斗人物 pic，`v2` 贴图所属加载文件 id（`lib.PicLoadFile` 的 id），`v3` 武功效果 pic  
  `x,y`：战斗人坐标。

### 二进制字节数组 `Byte`

Lua 对二进制文件支持有限，故提供以下函数：

| 函数 | 说明 |
|------|------|
| `Byte.create(size)` | 创建大小为 `size` 的字节数组 |
| `Byte.loadfile(b, filename, start, length)` | 从文件 `filename` 的 `start` 起读 `length` 字节到 `b`（`start` 从 0 起） |
| `Byte.savefile(b, filename, start, length)` | 将 `b` 写入文件 |
| `Byte.get16(b, start)` / `Byte.set16(b, start, v)` | 有符号 16 位整数 |
| `Byte.getu16(b, start)` / `Byte.setu16(b, start, v)` | 无符号 16 位整数（常用于人物经验） |
| `Byte.get32(b, start)` / `Byte.set32(b, start, v)` | 有符号 32 位整数 |
| `Byte.getstr(b, start, length)` / `Byte.setstr(b, start, length, str)` | 读写定长字符串 |
