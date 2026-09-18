# PEEK 四大软件深度详解

---
title: 🛠️工具链·PEEK 四大软件深度详解
---

# 
  PEEK 四大软件深度详解 — 从头骨 CT 到 3D 模型的每一步

> 
  
    入库日期：2026-08-23 · 来源：魅惑指示逐软件深挖 · 分类：PEEK 海外推广 / 设计工具链技术细节\
    覆盖：PROPLAN CMF / Mimics / Geomagic Studio·Freeform / 3-matic + Magics\
    附核心问题解答：头骨 CT 是怎么变成 3D 模型的？用什么软件？数据格式是什么？
  

## 
  〇、先回答核心问题：CT 怎么变成 3D 模型？

### 
  数据格式链

  ```
  CT 扫描仪输出：DICOM（.dcm 文件序列）
     ↓【Mimics】导入转换
  Mimics 项目文件（.mcs）内含三维体素场（每个体素带 HU 值）
     ↓ 阈值分割 + 区域生长 + Calculate 3D（marching cubes 表面提取）
  STL（binary 二进制格式，行业交换标准）
     ↓【Geomagic / 3-matic / Magics】修补·设计·验证
  最终 STL → 切片软件 G-code → PEEK 打印机
  ```

### 
  关键概念：Hounsfield 单位（HU）

  CT 图像每个像素是一个 HU 值=X 射线密度测量（水=0，空气=-1000，密质骨可达 +1000 以上）

  Mimics 的"阈值分割"就是按 HU 区间筛体素：预设 Bone (CT) 阈值下限默认 226，上限随骨密度浮动（文献常见 226–1405）

  阈值太低→骨头分不开；太高→出现假性骨缺损——需要人工微调

### 
  Mimics 内部完整步骤（文献逐步协议实测）

  File → New Project Wizard → 选 DICOM 目录 → Convert（软件把断层图像转成三维体素场）

  确认方向标记（T顶/B底/A前/P后/L左/R右）

  Segment → Thresholding → 选 Bone (CT) 预设 → 微调 Min/Max → Apply（生成 Mask 掩膜）

  Region Growing（区域生长）：点一下目标骨区，凡与种子点连通且在阈值内的体素归为一个对象（用于把下颌骨和颅骨分开等）

  Morphology Operations（形态学开闭运算去毛刺填小洞）

  Calculate 3D from Mask（质量选 Optimal）→ 生成三角面片网格

  File → Export → Binary STL

  全流程熟练者约 1 天内完成单病例分割（行业节奏：数据采集 0.5-1 天 + 分割 1 天 + 设计审阅 1-2 天 + 验证 1 天）

## 
  一、PROPLAN CMF（手术规划与数据导出）

  
    
      
        项
      

    

    
      
        细节
      

    

  

  
    
      
        厂商
      

    

    
      
        Materialise（CE 认证 L-102605-02），前身 SurgiCase CMF
      

    

  

  
    
      
        定位
      

    

    
      
        颅颌面（CMF）手术虚拟规划平台——正颌/下颌与中面部重建/截骨/牵引成骨
      

    

  

  
    
      
        核心功能
      

    

    
      
        患者 2D/3D 解剖可视化；截骨与骨块重定位模拟；软组织模拟（预测术后脸型）；牙模/植骨区扫描+面部照片融合计划；3D 头影测量分析；术后结果分析
      

    

  

  
    
      
        输出
      

    

    
      
        虚拟咬合板（splint）设计与导出 3D 打印；解剖模型导出
      

    

  

  
    
      
        双平台架构
      

    

    
      
        PROPLAN CMF（通常由 Materialise 临床工程师在直播互动会话中操作，医生无需安装软件知识）+ PROPLAN CMF Online（网页端：医生创建病例/上传 CT 数据交换/追踪查看手术方案）
      

    

  

  
    
      
        生态
      

    

    
      
        与 DePuy Synthes TRUMATCH CMF 解决方案配套（颅骨重建/正颌/牵引器定位）
      

    

  

  
    
      
        系统要求
      

    

    
      
        低配即可：Intel i3 / 4GB RAM / 256MB 显卡
      

    

  

  
    
      
        在链路中的角色
      

    

    
      
        数据入口：手术规划定稿后由 PROPLAN CMF 导出数据，交给下游设计（魅惑实战确认）
      

    

  

## 
  二、Mimics（CT → 3D 模型的转换核心）

  
    
      
        项
      

    

    
      
        细节
      

    

  

  
    
      
        全称
      

    

    
      
        Materialise's Interactive Medical Image Control System
      

    

  

  
    
      
        版本参考
      

    

    
      
        v19-v26（EU MDR 论文用 v25.0 颅骨例/v26.0 面部例）；官方认证课 Mimics Fundamentals $440
      

    

  

  
    
      
        输入
      

    

    
      
        DICOM（CT/MRI，要求高分辨率各向同性或近各向同性体素可大幅减少分割工作量；临床 CMF 标准：层厚 0.5mm/512×512 矩阵/骨算法重建）
      

    

  

  
    
      
        核心工具
      

    

    
      
        Thresholding（HU 阈值）、Region Grow、Split Mask、Edit Masks（Lasso）、Local Threshold（低对比区）、Multiple Slice Edit（自动插值）、Calculate Part/3D
      

    

  

  
    
      
        输出
      

    

    
      
        Binary STL（直接对接 3-matic/打印）；项目存 .mcs
      

    

  

  
    
      
        进阶技巧
      

    

    
      
        Mask 3D 预览勾选 Limit number of shells 去浮点；Wrap 包裹生成水密面（最小细节设为与像素同尺寸如 0.5mm）；平滑系数 0.7
      

    

  

## 
  三、Geomagic Studio / Freeform（补洞 + 触觉雕塑）

### 
  Geomagic Studio（补洞/逆向表面重建）

  定位：多边形处理与 NURBS 逆向——"补洞"主力

  Mesh Doctor 自动激活：修高度褶皱边/尖刺等缺陷

  补洞：Polygons → Fill Single（删面后补孔）；Exact Surfacing 精确曲面模式 → Detect Contours 按曲率/功能区分区 → 生成参数曲面

  学术界标准用法：Mimics 出 STL → Studio 补洞光顺 → 转 STEP 给 ANSYS 有限元（文献协议：v2012+）

### 
  Geomagic Freeform / Plus（触觉雕塑——行业版 ZBrush）

  
    
      
        项
      

    

    
      
        细节
      

    

  

  
    
      
        厂商
      

    

    
      
        3D Systems / Hexagon
      

    

  

  
    
      
        特色
      

    

    
      
        配 Touch / Touch X 触觉设备（6DOF 力反馈笔，电机反推手掌模拟"摸到"虚拟粘土）
      

    

  

  
    
      
        建模范式
      

    

    
      
        五种混合：Clay 粘土（变形浮雕）/ SubD（平滑面+锐边）/ NURBS+实体（精确 CAD）/ 网格
      

    

  

  
    
      
        StructureFX™
      

    

    
      
        定制内外晶格结构——医疗植入物功能构架专用（配合 3D 打印分析）
      

    

  

  
    
      
        专利技术
      

    

    
      
        三维像素（voxel）拓扑零错误保证——省掉几何修复成本
      

    

  

  
    
      
        文件格式
      

    

    
      
        输入：STL/OBJ/CAD/扫描任意来源；输出：STL(binary/ASCII)/OBJ/PLY/IGES/STEP/Parasolid（8.0 起）；Plus 版另支持更多 CAD 格式并含 Geomagic Wrap
      

    

  

  
    
      
        渲染
      

    

    
      
        内置 KeyShot 高分辨率渲染
      

    

  

  
    
      
        医疗应用
      

    

    
      
        颅颌面定制植入物/手术导板/矫形器（患者扫描数据→贴合设计）
      

    

  

## 
  四、3-matic（网格级设计优化）

  
    
      
        项
      

    

    
      
        细节
      

    

  

  
    
      
        定位
      

    

    
      
        网格层级的设计修改优化——CAD 设计/拓扑优化模型/仿真/扫描数据的粗糙数据清理
      

    

  

  
    
      
        核心能力
      

    

    
      
        Wall Thickness 壁厚分析热图（判定薄结构可打印性；超限区域只显示最大值；可离散成色带）；Measure Analysis Locally 局部测厚；Trim 修剪；Remesh 重划网格
      

    

  

  
    
      
        模块
      

    

    
      
        Texturing（STL 上直接加纹理/穿孔/图案——切片级技术避免文件爆炸）/ Lightweight Structures（轻量晶格）/ CAD Link（网格识别解析形状转全参数 CAD）/ Scripting（Python API 自动化）
      

    

  

  
    
      
        与 Mimics 协作
      

    

    
      
        Mimics 项目直接 Ctrl+C/V 粘贴进 3-matic（无缝）
      

    

  

  
    
      
        系统要求
      

    

    
      
        Win10 64bit；推荐 i5/i7 + 16GB RAM + 2GB 显卡
      

    

  

## 
  五、Magics（打印前验证与构建准备——行业标杆）

  
    
      
        项
      

    

    
      
        细节
      

    

  

  
    
      
        定位
      

    

    
      
        "最强 3D 打印数据和构建准备软件"——技术中立（SLS/MJF/EBM/SLA/DLP/金属砂粘结/FDM/FFF 全支持）
      

    

  

  
    
      
        验证能力
      

    

    
      
        STL 修复（翻转三角/坏边/孔洞/非流形边）；壁厚分析（强度+可打印性+螺孔直径）；碰撞检测；切片预览
      

    

  

  
    
      
        构建准备
      

    

    
      
        零件复制/最优摆放/支撑生成（SG+ 金属模块防翘曲脱板；e-Stage 自动支撑）/Nester 嵌套排样/无建造区
      

    

  

  
    
      
        模块家族
      

    

    
      
        SG+（金属支撑）/ Ansys Simulation（打印前仿真避废件）/ Lattice（晶格减重）/ Nester / Dental / Import（几乎所有 CAD 格式）
      

    

  

  
    
      
        自动化
      

    

    
      
        工作流自动化（支撑/嵌套/标签/仿真）+ 可定制自动报告；CO-AM 平台与 Build Processors 集成
      

    

  

  
    
      
        行业地位
      

    

    
      
        全球公司数据准备首选；独立于打印机品牌——一套软件管所有机器
      

    

  

## 
  六、全链路数据格式速查表

  ```
  DICOM (.dcm 序列)     ← CT/MRI 原始输出（含患者信息+像素矩阵+HU值）
    ↓ Mimics
  .mcs 项目             ← Mimics 工程（掩膜+体素+3D对象）
    ↓ Calculate 3D
  STL (binary)          ← 三角面片，行业交换标准（Freeform/3-matic/Magics 通吃）
    ↓ Geomagic Studio   （可出 STEP/IGES 参数曲面给 FEA/CAD）
  OBJ / PLY             ← 备选网格格式（带颜色/纹理时用 OBJ）
  STEP / IGES / Parasolid ← CAD 参数格式（Freeform Plus / 3-matic CAD Link 输出）
  G-code                ← 切片软件（Cura 等）生成的打印机指令
  ```

KS_DOC_REVIEWS	xijTZQtz2LOom4UeaBtqZa	22832	https://www.workbuddy.cn/space/d/xijTZQtz2LOom4UeaBtqZa