# OKF·李书远数据库使用手册与创作体系

---
title: 📋制度·李书远数据库使用手册与创作体系
---

  okf\_version: "0.1"\
  type: bundle\_index\
  title: 李书远数据库使用手册\
  description: 国内医美自媒体运营素材库（骨相审美文案 / PEEK 素材 / 朋友圈话术）+ 强制视频处理标准（>100MB 文件必须分割到 ≤100MB 再上传）+ 本地/云端双载体使用规范；raw/ 收录创作体系全文（骨相文案集/语感基准/创作要求/SOP/书单/CT-DICOM 技术标准）\
  tags: \[lishuyuan, 李书远, 医美, 自媒体运营, 骨相审美, PEEK, 视频处理, media-ops, 朋友圈文案, 语感基准, 创作要求, 书单, CT-DICOM\]\
  trigger: 李书远数据库, 李书远, 国内医美自媒体运营, 医美运营素材, 骨相审美文案, 骨相审美, 语感基准, 创作要求, PEEK素材, 视频处理标准, 上传分割, 朋友圈更新文案, CT-DICOM标准, 医美书单, 团队SOP\
  related:

  F:codexokf-bundlesdomestic-social-media-opsindex.md（国内社媒运营 SOP）

  F:codexokf-bundlespeek-medical-aestheticindex.md（PEEK 医美材料知识）

  F:codextoolsagent-system-mapcatalogdatabase-registry-v0.json（权威数据库注册表 db-domestic-aesthetic-media-ops）

  F:codexdatadomestic-aesthetic-media-ops（本地库文件）\
  fresh\_until: 2026-11-15（+90d）

> 
  
    坐标：map.projects / projects.new-media-ops（新媒体运营）\
    呈现：manual-registry（okf:lishuyuan-database）
  

# 
  李书远数据库使用手册

## 
  一句话规则

  ```text
  李书远数据库 = 国内医美自媒体运营素材库（骨相审美理念 → PEEK 落地业务）；
  双载体 = 本地 F:\codex\data\domestic-aesthetic-media-ops\ + WorkBuddy 资料库云端空间；
  视频标准 = 任何 >100MB 文件必须先分割到 ≤100MB 再上传（FFmpeg 按时长法，-segment_size 不可用）；
  搜索词 = 李书远 / 国内医美自媒体运营 / 骨相审美 / PEEK / 医美运营素材。
  ```

## 
  1. 库定位

  性质：国内医美自媒体运营的创作素材库（短视频文案、朋友圈文案、账号简介、话术、风格基准）

  主题关系：骨相审美 = 理念/内容层（立人设与审美认知）；PEEK（聚醚醚酮）= 落地业务层（3D 打印骨相填充/轮廓手术推广）。创作顺序：骨相文案开场 → PEEK 科普/对比承接转化

  载体：本地 Markdown 文件库 + 云端资料库空间（同一库的双载体）

## 
  2. 双载体与访问

  
    
      
        载体
      

    

    
      
        位置
      

    

    
      
        用途
      

    

  

  
    
      
        本地库
      

    

    
      
        `F:\codex\data\domestic-aesthetic-media-ops\`
      

    

    
      
        权威文件库（raw/素材/需求/README）
      

    

  

  
    
      
        云端空间
      

    

    
      
        WorkBuddy 资料库（8 篇文档 + 任务表 + 视频处理标准）
      

    

    
      
        团队/多端查看，上传素材
      

    

  

  
    
      
        注册表
      

    

    
      
        `agent-system-map\catalog\database-registry-v0.json` db-domestic-aesthetic-media-ops
      

    

    
      
        Agent 预判器可命中
      

    

  

## 
  3. 目录结构（本地库）

  ```
  domestic-aesthetic-media-ops/
  ├── README.md              # 使用说明（李书远命名 + 视频标准）
  ├── raw/                   # 原始素材存档（只读归档）
  ├── 需求/                  # 创作要求/需求规格
  └── 素材/                  # 整理后文案条目
      ├── 骨相审美/          # 骨相审美文案集（GM-骨相-001~010）
      ├── PEEK/              # PEEK材料知识/文案关键词与卖点/平台账号源/结构化数据
      └── 风格基准/          # 语感规范
  ```

## 
  4. 视频处理标准（强制，所有上传者遵守）

> 
  
    凡是超过 100MB 的任何文件，都必须分割到 100MB 以内（建议 ≤90MB 留余量）才能上传。\
    完整细节见云端空间「视频处理标准」文档；摘要如下：
  

  判定：文件 >100MB → 必须分割

  分割方法：FFmpeg 按时长法（`-segment_size` 在 gyan.dev 版不可用，实测 8.1/9.0.1 报 Unrecognized option）：

  
    `ffprobe` 探测总时长/大小 → 算平均码率 → 每段时长 = 90MB ÷ 码率
  

  
    `ffmpeg -i input.mp4 -c copy -f segment -segment_time  -reset_timestamps 1 out_%03d.mp4`
  

  校验：每段 ≤100MB（建议 ≤90MB）

  备注规范：每段备注必须写原文件名、总大小、分片数、本片序号、分割时间、工具版本（如 FFmpeg 9.0.1）

  禁止：>100MB 原样上传 / 用 `-segment_size` / 未经同意重新编码降质

## 
  5. 三级坐标

  
    
      
        层级
      

    

    
      
        坐标
      

    

    
      
        含义
      

    

  

  
    
      
        一级
      

    

    
      
        `map.projects`
      

    

    
      
        项目与资产面板
      

    

  

  
    
      
        二级
      

    

    
      
        `projects.new-media-ops`
      

    

    
      
        新媒体运营子域
      

    

  

  
    
      
        三级
      

    

    
      
        （库不挂 L3；视频素材维度由 `dac:shipin` 数字资产·视频承接）
      

    

    
  

  注册：权威 `database-registry-v0.json`（db-domestic-aesthetic-media-ops，2026-08-17）

  验证：预判器 `suggest_task_context.py --subnode projects.new-media-ops` 命中

  历史修正：原坐标 `map.projects.medical-aesthetic.media-ops`（`medical-aesthetic` 不在合法子域值域）

## 
  6. 使用规范（写库必读）

  条目字段：`GM--` + 原文/主题/用途/时长/入库日期/来源

  用户原文必须原样归档到 `raw/` 再整理

  风格基准见 `素材/风格基准/`

  与医美语料库（medical-aesthetic-corpus）互补：本库管"表达"，语料库管"医学事实/合规依据"

## 
  7. 关联

  `okf:domestic-social-media-ops`：国内社媒运营 SOP（方法层）

  `okf:peek-medical-aesthetic`：PEEK 医美材料知识包

  `db-002 医美语料库`：行业知识语料库

  任务表：云端「医美运营任务表」（每天任务/单次任务，板块/任务模式/状态/优先级/负责人）

# 
  创作体系全文收录（2026-08-23 OKF 化增补）

> 
  
    来源：李书远本地库 `素材/`+`需求/`（raw/ 全文收录，权威原件仍在本地库）\
    fresh\_until: 2026-11-21（+90d）
  

## 
  raw 文件清单

  
    
      
        类别
      

    

    
      
        文件
      

    

    
      
        用途
      

    

  

  
    
      
        创作理念
      

    

    
      
        `raw/2026-08-14-骨相审美-文案集.md`
      

    

    
      
        骨相审美文案 10 条（GM-骨相-001~010），立人设核心素材
      

    

  

  
    
      
        创作理念
      

    

    
      
        `raw/骨相审美-语感基准.md`
      

    

    
      
        语感规范——所有文案的风格基准
      

    

  

  
    
      
        创作理念
      

    

    
      
        `raw/2026-08-14-骨相审美-创作要求.md`
      

    

    
      
        用户创作要求规格
      

    

  

  
    
      
        规范
      

    

    
      
        `raw/CT-DICOM数据接收与处理标准-20260819.md`
      

    

    
      
        CT 数据接收/处理技术标准（配合 PEEK 设计管线）
      

    

  

  
    
      
        SOP
      

    

    
      
        `raw/李书远数据库团队SOP.md`
      

    

    
      
        团队协作八大章（数据入口/入库规范/看板维护/agent 工作流）
      

    

  

  
    
      
        工具参考
      

    

    
      
        `raw/WorkBuddy资料库应用参考.md`
      

    

    
      
        资料库高级玩法参考（三件套/关联数据/发布网站）
      

    

  

  
    
      
        书单
      

    

    
      
        `raw/医学院头部解剖教科书书单-20260817.md` 等 4 份
      

    

    
      
        医美书架采购清单（头部解剖/颅骨专题/专业书单）
      

    

  

## 
  检索提示

  写骨相文案 → 先读语感基准再套文案集条目格式

  PEEK 设计相关技术标准 → 配合 `okf:peek-medical-aesthetic` 的设计管线章节

  团队新成员/新 agent 入职 → 必读 SOP 全文

KS_DOC_REVIEWS	muZOC3k2kFKBKu9IrVSUQg	17707	https://www.workbuddy.cn/space/d/muZOC3k2kFKBKu9IrVSUQg