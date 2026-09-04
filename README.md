

# Customer Segmentation & Value Analysis

Segmenting 2,212 retail customers with K-Means, then converting the segments
into marketing decisions a budget owner could act on.

**Dataset:** Marketing Campaign (2,240 records, 2,212 after cleaning)
**Notebook:** `customer_segmentation.ipynb`

---

## Headline Findings

**1. Revenue is heavily concentrated.**
Cluster 0 holds **23.0% of customers and generates 51.9% of total spending** —
2.26x its share of the base. Clusters 0 and 1 together are 49% of customers
and 90% of revenue.

**2. Spending varies far more than income does.**
Average income spans **2.5x** across segments. Average spending spans
**12.7x**. Income is not what separates high-value customers — household
composition and channel preference are. Cluster 0 and Cluster 2 differ 1.8x in
income but 10x in spending, and the sharpest difference between them is
children (0.03x vs 1.95x the average).

**3. The most valuable customers are the most responsive and the least
discounted.**
Cluster 0 accepts campaigns at **43.3%** against an overall rate of **20.7%**,
while using promotional deals **least** of any segment (0.49x average).
Discount spend is going where it is least needed.

---

## Business Problem

One marketing strategy applied to every customer wastes budget on people who
will not respond and under-serves the customers who generate most of the
revenue. This project asks: what groups exist, how large is each, where does
revenue concentrate, and what specific action follows for each group?

---

## Approach

```
Cleaning → Feature engineering → Encoding → Scaling
    → Choosing k (elbow + silhouette) → K-Means → Stability check
    → Segment profiling → Business interpretation
```

### Feature engineering

Derived: total spending across six product categories, age, household size,
number of children, parenthood flag, customer tenure, living situation.

**Education was mapped to an explicit ordinal scale rather than label-encoded.**
`LabelEncoder` assigns codes alphabetically, which ranks Undergraduate (2)
above Postgraduate (1). Feeding that false ordering into a distance-based
algorithm distorts every cluster.

### Choosing k — and why the metric was overruled

| k | Inertia | Silhouette |
|---|---------|------------|
| 2 | 36,642 | **0.274** |
| 3 | 32,104 | 0.210 |
| 4 | 29,687 | 0.157 |
| 5 | 28,488 | 0.145 |
| 6 | 27,118 | 0.149 |

Silhouette peaks at **k=2** and declines from there. The data does not contain
four well-separated groups; it contains a spending gradient that k=2 splits
most cleanly into high and low spenders.

**k=4 was selected anyway.** Two reasons:

- The four-way split is **perfectly reproducible** — adjusted Rand index of
  **1.000 across five random seeds**. These boundaries are not an artefact of
  initialisation.
- k=2 is not actionable. It would group a 76k-income customer who buys without
  discounts together with a 62k-income customer who buys mainly on deals.
  Those two need opposite treatment.

Statistical separation was traded for operational usefulness. That trade-off is
stated here rather than hidden.

---

## Segments

| Cluster | Profile | Customers | Share | Revenue share | Avg income | Avg spend |
|---|---|---|---|---|---|---|
| **0** | High value | 508 | 23.0% | **51.9%** | 76,173 | 1,373 |
| **1** | Active mid-to-high value | 585 | 26.4% | 38.1% | 61,731 | 874 |
| **2** | Family-oriented, low spending | 510 | 23.1% | 5.1% | 42,653 | 135 |
| **3** | Low value, low engagement | 609 | 27.5% | 4.9% | 30,166 | 108 |

Indexed against the overall average (1.00 = average):

| Feature | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| Income | 1.47 | 1.19 | 0.82 | 0.58 |
| Spending | **2.26** | 1.44 | 0.22 | 0.18 |
| Children | **0.03** | 1.19 | **1.95** | 0.83 |
| Deal purchases | **0.49** | **1.49** | 1.19 | 0.80 |
| Catalog purchases | **2.24** | 1.37 | 0.29 | 0.20 |
| Web visits | 0.50 | 1.02 | 1.14 | **1.29** |

*Cluster numbers are run-specific. Changing preprocessing renumbers the groups —
read the profile, not the label.*

---

## Web Conversion

| Cluster | Visits/month | Web purchases | Purchases per visit |
|---|---|---|---|
| 0 | 2.65 | 4.94 | **1.87** |
| 1 | 5.42 | 6.61 | 1.22 |
| 2 | 6.05 | 2.50 | 0.41 |
| 3 | 6.84 | 2.29 | **0.33** |

Visit frequency and conversion move in opposite directions — a **5.6x gap**
between the best and worst segments.

**One caveat before acting on this.** The straightforward reading is a broken
funnel losing ready buyers. But frequent visits with few purchases is equally
the signature of browsing and deal-hunting, where the visits reflect
unwillingness to pay rather than friction at checkout. The two readings imply
opposite actions and this data cannot separate them. A holdout test on a
targeted offer would.

---

## Recommended Actions

| Segment | Action | Rationale |
|---|---|---|
| **C0** High value | Retention, exclusivity, service — **not discounts** | 52% of revenue; 43.3% campaign acceptance; already buys without deals (0.49x) |
| **C1** Active mid-high | Loyalty and cross-sell; test moving toward C0 behaviour | 38% of revenue; heaviest deal user (1.49x) — currently price-driven |
| **C2** Family-oriented | Family bundles, volume pricing | Constraint is household budget (1.95x children), not disengagement |
| **C3** Low value | Holdout test before investing | 28% of customers, 4.9% of revenue |

### Sizing the conversion opportunity honestly

Cluster 3 holds 609 customers (27.5% of the base) contributing 4.9% of
revenue. A 10% lift in their spending would raise total revenue by **0.49%**.

That is small, and saying so is the point. The larger lever sits in Cluster 1:
it holds 38% of revenue and is the heaviest user of discounts, so margin
recovery and upward movement there move more money than anything achievable in
the low-value segments.

---

## Limitations

- **Separation is modest.** Silhouette 0.157 at k=4 against 0.274 at k=2. The
  structure is a gradient, not distinct groups. Boundaries are analytical
  conveniences — stable ones (ARI 1.000), but conveniences.
- **The conversion gap is correlational**, and fits two opposite explanations.
- **Campaign response is historical.** No campaign was run against these
  segments, so rates describe general responsiveness, not expected targeting
  performance.
- **Segments are a snapshot.** Migration between groups is not tracked.
- **Education was mapped ordinally**, assuming comparable gaps between levels.
- **The revenue-uplift figure rests on an assumed lift**, not a measured one.

## Next Steps

- Test whether Cluster 1 customers can be moved toward Cluster 0 behaviour —
  that is where the revenue is.
- Run a targeted offer on Cluster 3 against a holdout to separate the
  funnel-friction reading from the deal-hunting one.
- Reallocate discount spend away from Cluster 0 and measure the margin effect.
- Track segment migration across periods.
- Compare against Gaussian mixture models, which handle gradient-like structure
  better than centroid methods.

---

## Tech Stack

Python · Pandas · NumPy · Scikit-learn (K-Means, PCA, StandardScaler) ·
Matplotlib · Seaborn · Google Colab

## Running It

```bash
pip install pandas numpy scikit-learn matplotlib seaborn gdown
jupyter notebook customer_segmentation.ipynb
```

The notebook looks for `marketing_campaign.csv` locally first and downloads it
if absent. Run all cells in order.

---

# 日本語概要

## 顧客セグメンテーション分析

小売顧客 2,212 名を対象に、収入・世帯構成・商品カテゴリ別支出・購買チャネルを
用いて K-Means クラスタリングを実施し、分析結果をマーケティング施策に接続した
プロジェクトです。

### 主な発見

**1. 売上は特定セグメントに集中している**
クラスタ0は顧客全体の **23.0%** でありながら、売上の **51.9%** を生み出して
います（構成比の 2.26 倍）。クラスタ0と1を合わせると、顧客の 49% で売上の 90%
を占めます。

**2. 支出の格差は収入の格差をはるかに上回る**
セグメント間の平均収入の差は **2.5倍** であるのに対し、平均支出の差は
**12.7倍** に達しました。顧客価値を左右する主要因は収入そのものではなく、世帯
構成と購買チャネルの選好にあります。クラスタ0と2は収入で 1.8 倍の差ですが支出
では 10 倍の差があり、両者を最も分けている要因は子どもの数（平均比 0.03倍 対
1.95倍）でした。

**3. 最も価値の高い顧客は、最も反応が良く、かつ最も値引きを必要としていない**
クラスタ0のキャンペーン受諾率は **43.3%**（全体平均 20.7%）である一方、割引
利用は全セグメント中最も低い（平均比 0.49倍）。値引き予算が、最も必要とされて
いない層に投下されている可能性を示しています。

### 手法上の工夫

**Education は LabelEncoder ではなく順序尺度として明示的にマッピング**
LabelEncoder はアルファベット順にコードを割り当てるため、Undergraduate が
Postgraduate より上位になってしまい、距離ベースのアルゴリズムに誤った順序を
与えることになります。

**クラスタ数は指標を「あえて採用しなかった」判断を明示**
シルエット係数は k=2（0.274）が最大で、k=4 では 0.157 まで低下します。データは
明確に分かれた4群ではなく支出の連続的なグラデーションであると解釈できます。

それでも k=4 を採用した理由は二点あります。第一に、調整ランド指数が5つの乱数
シードすべてで **1.000** であり、この分割が初期値に依存しない再現性のあるもの
だからです。第二に、k=2 では「割引なしで購入する高収入層」と「割引主導で購入
する中収入層」が同一グループになり、正反対の施策が必要な顧客を区別できないため
です。統計的な分離度を犠牲にして実務上の有用性を優先した判断であることを、
README 上で明示しています。

### 施策提案

| セグメント | 施策 | 根拠 |
|---|---|---|
| C0 高価値層 | 維持・特別待遇・サービス（**値引きではなく**） | 売上の52%、受諾率43.3%、割引利用0.49倍 |
| C1 中～高価値層 | ロイヤルティ・クロスセル、C0への移行検証 | 売上の38%、割引利用1.49倍で価格主導 |
| C2 ファミリー層 | ファミリーバンドル・数量価格 | 制約は世帯予算（子ども1.95倍）であり無関心ではない |
| C3 低価値層 | 投資前にホールドアウト検証 | 顧客の28%だが売上は4.9% |

分析を「4つに分類した」で終わらせず、各セグメントに対して実行可能な施策と、
その施策が狙う目的、および投資判断の根拠まで接続しています。

### 限界の明示

クラスタの分離度は高くありません（k=4 でシルエット 0.157）。また、訪問頻度と
購入率の乖離は相関に過ぎず、「購入導線に問題がある」という解釈と「価格に納得
していないだけ」という解釈の双方が成立します。両者は正反対の施策を導くため、
本データのみでは判別できないことを記載した上で、ホールドアウトを用いた検証を
次のステップとして提示しています。
