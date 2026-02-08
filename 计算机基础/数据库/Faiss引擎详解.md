---
title: Faiss 深度技术解析：向量检索的算法基石
created_at: 2026-02-08
updated_at: 2026-02-08
tags: [Faiss, 向量检索, 算法, MetaAI]
---

# Faiss 深度技术解析：向量检索的算法基石

Faiss (Facebook AI Similarity Search) 是 Meta 开发的专门用于稠密向量相似性搜索的开源库。它是许多向量数据库（如 Milvus、OpenSearch）的底层动力源。

## 1. 核心架构与原理

Faiss 的核心目标是在给定的 $D$ 维向量集合 $S$ 中，快速找到与查询向量 $q$ 距离最近的 $k$ 个向量。

### 1.1 核心算法：IVF + PQ
为了处理亿级数据，Faiss 常用 **IVF (倒排索引)** 与 **PQ (乘积量化)** 的组合：
1.  **IVF (分桶)**：将向量空间聚类，查询时只搜索相关的聚类桶。
2.  **PQ (压缩)**：将 128 维的浮点数向量压缩为 8 字节或 16 字节的短编码，极大节省内存。

## 2. 常用索引类型对比

| 索引名称 | 标识符 | 内存占用 | 搜索速度 | 准确度 |
| :--- | :--- | :--- | :--- | :--- |
| **精确搜索** | `IndexFlatL2` | 极高 | 慢 (线性扫描) | 100% |
| **倒排文件** | `IndexIVFFlat` | 高 | 快 | 良好 |
| **乘积量化** | `IndexIVFPQ` | **极低** | **极快** | 一般 (有损压缩) |
| **图索引** | `IndexHNSWFlat` | 高 | 极快 | 极高 |

## 3. 为什么 Faiss 如此快？
*   **数学优化**：底层利用了 BLAS 等高性能线性代数库。
*   **硬件加速**：
    *   **CPU**: 使用 SIMD (AVX2) 向量化指令。
    *   **GPU**: 专门优化的 CUDA 内核，支持单机多卡并行计算。
*   **索引预构建**：在搜索前构建复杂的索引结构（如 HNSW 图），将搜索复杂度从 $O(N)$ 降低到 $O(\log N)$。

## 4. 与向量数据库的关系
Faiss 相当于**汽车的引擎**。
而 Milvus/OpenSearch 等数据库相当于**整车**（提供了座椅、空调、方向盘，即分布式、持久化、API 接口）。

## 5. 使用场景
*   **推荐系统**：根据用户 Embedding 实时查找相似商品。
*   **视觉搜索**：以图搜图，处理亿级图片特征。
*   **RAG (检索增强生成)**：作为本地 Embedding 知识库的检索引擎。

## 参考链接
- [Faiss GitHub Repository](https://github.com/facebookresearch/faiss)
- [Faiss 官方教程](https://github.com/facebookresearch/faiss/wiki/Getting-started)

## Update History
- 2026-02-08: 初次创建，解析 Faiss 核心原理及索引类型。
