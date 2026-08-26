# 李书远数据库团队SOP

---
title: 🛠️工具链·李书远数据库团队SOP
---

# 
  李书远数据库 · 团队使用 SOP

> 
  
    状态：生效（2026-08-18）\
    适用范围：所有使用李书远数据库的 Agent / 团队成员\
    目的：统一"存→管→用→看→协"的规范，让库持续可用、数据不丢、看板常新\
    关联：`素材/WorkBuddy资料库应用参考.md`、`素材/PEEK/` 系列、看板 2 个
  

## 
  一、库结构总览

  
    
      
        载体
      

    

    
      
        位置
      

    

    
      
        用途
      

    

  

  
    
      
        本地权威库
      

    

    
      
        `F:\codex\data\domestic-aesthetic-media-ops\`
      

    

    
      
        原始文件（raw / 素材 / 需求 / README）
      

    

  

  
    
      
        云端空间
      

    

    
      
        WorkBuddy 资料库（35+ 节点）
      

    

    
      
        团队共享、在线查看、协作
      

    

  

  
    
      
        数据表
      

    

    
      
        PEEK市场数据（FYFcrIsyKj9IavloAVGXQd）、医美运营任务表（glqvIFb5dyy2kNicdbpSFp）
      

    

    
      
        结构化数据
      

    

  

  
    
      
        看板
      

    

    
      
        任务看板、PEEK 数据看板（已发布）
      

    

    
      
        团队可视化查看
      

    

  

  命名：李书远数据库（国内医美自媒体运营）= 本地 + 云端同一库

## 
  二、数据入口流程（公众号/链接等）

  任何外部内容进入，先走 intake，禁止直接问"怎么处理"：

  保存归档：公众号链接 → `C:\HTML MD 网络来源\微信公众号\`（命名：日期\_时间\_文章ID\_标题，HTML+MD 双份）

  分流判断：

  
    进数据库：与 PEEK/医美内容域相关 → 存本地 raw/素材 → 同步云端
  

  
    变任务：有明确工程动作 → beads 建任务
  

  
    仅归档：无关联内容 → 保留归档即可
  

  记录：在 beads 追加 intake 记录（map.beads.intake，intake:wechat）

## 
  三、素材入库规范

### 
  3.1 原始文件（raw）

  用户原文 / 抓取原文 → `raw/` 按日期分目录，只读归档

  文献原文 → `raw/-PEEK文献原文/`，标注来源/DOI/抓取时间

### 
  3.2 整理素材（素材/）

  按主题分目录（素材/PEEK/、素材/骨相审美/、素材/风格基准/…）

  每个文件头标注：主题、入库日期、用途、来源、关联

### 
  3.3 结构化数据（database）★ 重要

> 
  
    ⚠️ 必须用 CSV 导入，不要用批量添加接口\
    踩坑记录：`batch_add_database_records` 写入的记录查询接口读不到（索引不同步）；\
    正确路径：生成 CSV → `import_csv.py --token-stdin  --database-id ` 覆盖导入。
  

  建表：`create_database.py --schema '{"title":"...","space_id":"...","properties":[...]}'`（类型嵌在 config 里）

  导入：CSV（utf-8-sig 带表头）→ import\_csv

  字段设计参考：PEEK市场数据（分类/指标/数值/区域/年份/来源/解读）

## 
  四、看板维护

### 
  现有看板

  
    
      
        看板
      

    

    
      
        数据源
      

    

    
      
        链接
      

    

  

  
    
      
        医美运营任务看板
      

    

    
      
        任务表 glqvIFb5dyy2kNicdbpSFp
      

    

    
      
        [https://workbuddy.link/p/i2U4UBSzaM6H0sYNT6OY3U](https://workbuddy.link/p/i2U4UBSzaM6H0sYNT6OY3U)
      

    

  

  
    
      
        PEEK 市场数据看板
      

    

    
      
        PEEK市场数据 FYFcrIsyKj9IavloAVGXQd
      

    

    
      
        [https://workbuddy.link/p/Ps4TCCPYfHZMOSm37hY8Cn](https://workbuddy.link/p/Ps4TCCPYfHZMOSm37hY8Cn)
      

    

  

### 
  维护规则

  数据更新：改数据库 → 看板自动同步（SDK 双向绑定）

  看板改版：改本地 HTML → lint → `import_html.py --node-block-id  --databases ...` 覆盖 → 重新 publish-page

  lint 要求：含 addRecord 的页面必须有 localStorage 表单缓存（DSDK008）

### 
  看板 HTML 规范

  `data-sp-bindable="database"` + `data-sp-database-id=""` 必须成对

  用 `window.__SMART_PAGE__.database` SDK：query/getSchema/addRecord/updateRecord

  记录是扁平对象 `{字段: 值}`，含 record\_id

## 
  五、Agent 工作流（团队重点）

### 
  干活前

  引用空间：任务会话里引用李书远数据库空间（资料库直接进入上下文），不手动翻文件

  需要结构化数据 → 从 database 拉取（query）

### 
  干完活

  写回空间：新素材 → 本地入库 + 云端同步（doc/database）

  更新任务：看板/任务表里更新状态、备注

  记录进度：重要产出写 beads notes

### 
  数据表维护

  新增数据 → CSV 追加 → import\_csv 覆盖导入 → 看板自动更新

## 
  六、任务系统

  
    
      
        层
      

    

    
      
        工具
      

    

    
      
        说明
      

    

  

  
    
      
        日常任务
      

    

    
      
        医美运营任务表 + 任务看板
      

    

    
      
        每天/单次任务，看板可视化
      

    

  

  
    
      
        工程追踪
      

    

    
      
        beads（F:codex.beads）
      

    

    
      
        跨 agent 任务，坐标/优先级/状态
      

    

  

  
    
      
        进度记录
      

    

    
      
        beads notes
      

    

    
      
        重要节点写笔记，可追溯
      

    

  

  任务表字段：任务/板块/渠道·平台/任务模式/状态/优先级/负责人/截止日/备注

  看板操作：新增任务、点卡片改状态、筛选、统计——全部实时同步数据库

## 
  七、团队协作

  共享：云端空间对团队可见可编辑（公开链接发布后，链接即访问入口）

  多 agent：每个 agent 按本节工作流取数/写回，不另建孤岛

  发布：看板/页面发布为网站后，链接可分享给团队、客户

  权限：公开页面如需登录，检查空间公开访问设置

## 
  八、检查清单（每周维护时过一遍）

  云端节点与本地文件对账（无遗漏）

  任务表状态更新、看板显示正常

  PEEK 数据看板数据是否需更新（季度）

  新入库素材走完"raw→素材→云端"全流程

  公众号/链接 intake 归档无堆积

> 
  
    ⚠️ 本 SOP 随实践迭代；技术细节以资料库 skill 文档为准（`page/data-page-flow.md`、`database/entry.md`）。
  

KS_DOC_REVIEWS	i4NoCeXRVXPjcSJ7t9Igc6	15302	https://www.workbuddy.cn/space/d/i4NoCeXRVXPjcSJ7t9Igc6