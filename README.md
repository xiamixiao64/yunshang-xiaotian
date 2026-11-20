# 《云上小田》M1 玩法原型

小红书「小工具」方向的 3D 种田游戏可玩原型（对应策划案 M1 阶段）。

## 运行方式

需要通过 HTTP 服务访问（ES Module 限制，不能直接双击打开）：

```bash
cd prototype
python -m http.server 8420
# 浏览器打开 http://localhost:8420/index.html
```

建议用浏览器开发者工具切换到手机视口（竖屏 9:16）体验。

## 已实现内容

| 系统 | 说明 |
|---|---|
| 3D 场景 | Three.js 云端小岛：不规则岛缘、三层悬崖岩层、底部岩尖与垂根、环绕悬浮碎石、岛底云海、渐变天空（晨雾粉→天蓝）、ACES 色调映射 + 泛光辉光后期 |
| 专业模型 | CC0/CC-BY 低模资产（Quaternius、Google Poly 等，GLB 格式）：4 种花、樱花树、喷泉、长椅、栅栏、灯、灌木、水井（出处见下方署名） |
| 角色 | Q 版农夫小姑娘：脸部贴图（会眨眼）、草帽、围裙、洒水壶；会走到田边做播种下蹲 / 浇水洒水珠 / 收获跳跃动作 |
| 材质 | 程序化草地噪点纹理、土壤颗粒、木纹；300 株 GPU 实例化摇摆草叶（shader 顶点动画）；池塘/喷泉流动水面 shader |
| 音效音乐 | WebAudio 实时合成：全套操作音效 + 治愈系 BGM（和弦垫 C-Am-F-G + 五声音阶风铃），静音开关 |
| 种植 | 4 种花（郁金香 / 小雏菊 / 向日葵 / 玫瑰），生长动画 + 成熟星光提示 |
| 等级 | 经验系统：收获/卖出/交单/合成加经验，Lv2 小雏菊+订单、Lv3 向日葵、Lv4 玫瑰+工坊，升级烟花+音效 |
| 订单 | 小镇客人心愿单 ×2 并行，35% 溢价报酬+经验，可交付角标提醒 |
| 工坊 | 4 种花艺配方（花束/花篮/礼盒），成品卖价更高、订单抢手货 |
| 变异 | 10% 概率闪光变异 ×2 收获（金色粒子+专属音效） |
| 浇水 | 点击生长中的作物加速 38%，水滴坠落 + 涟漪特效 |
| 收获 | 收获星星粒子（带泛光） |
| 经营 | 背包 → 花店卖出，金币滚动动画 |
| 打扮 | 8 种装饰（气球束/奶油栅栏/月光路灯/木纹长椅/樱花树/小池塘/遮阳咖啡桌/星光喷泉/许愿水井），装饰度数值；路灯火光呼吸、池塘发光小莲花、喷泉水珠粒子 |
| 开场 | 标题画面「云上小田」，等待模型加载后可进入，点击即开始（同时解锁音频） |
| 存档 | localStorage 自动存档，离线作物继续生长（基于时间戳） |
| 防死锁 | 金币不足最便宜种子时，「邻居奶奶的礼物」送一粒随机种子 |
| 拍照 | 📸 按钮一键保存岛面截图（对齐小红书晒图传播） |
| 操作 | 点田播种/浇水/收获；按住拖动可环绕视角 |

## 模型素材署名（License）

全部素材允许商用；CC-BY 项需在发布页保留署名（正式发布时附在此列表即可）。

| 文件 | 作者 | 许可 |
|---|---|---|
| tulip.glb | jeremy (Poly Pizza) | CC-BY 3.0 |
| daisy.glb | Poly by Google | CC-BY 3.0 |
| sunflower.glb | Poly by Google | CC-BY 3.0 |
| rose.glb | Erbay ÇELIK (Poly Pizza) | CC-BY 3.0 |
| cherry.glb | Poly by Google | CC-BY 3.0 |
| fountain.glb | Isa Lousberg (Poly Pizza) | CC0 |
| bench.glb / fence.glb / lantern.glb / bush_flower.glb / hedge.glb / well.glb / farmer.glb | Quaternius / Kay Lousberg (Poly Pizza) | CC0 |

## 已验证的完整闭环

种 (扣金币) → 浇水加速 → 成熟收获 (进背包) → 花店卖出 (加金币) → 买装饰 (装饰度提升) → 装饰可见于岛上

## 文件结构

```
prototype/
├── index.html          # 全部游戏代码（单文件）
└── lib/
    ├── three.module.js       # Three.js r160（本地化，离线可跑）
    └── jsm/                  # 官方后期处理模块（泛光辉光）
        ├── postprocessing/   # EffectComposer / UnrealBloomPass 等
        └── shaders/          # CopyShader / OutputShader 等
```

> 注意：必须通过 HTTP 服务器访问（ES Module 不能从 file:// 打开）。已内置友好提示。

## 已知问题 / 下一步

- [ ] 装饰位目前固定 6 个位置，后续可做自由摆放
- [ ] 缺少音效与背景音乐
- [ ] 缺少截图分享按钮（小红书传播核心，M3）
- [ ] 小雏菊/向日葵/玫瑰模型还可精修
- [ ] 服务器目前是开发用 python http.server，发布到小红书 Builder Hub 时打包为静态资源上传
