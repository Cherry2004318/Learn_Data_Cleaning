# Online Retail 数据清洗与 RFM 客户价值分析

本项目基于 Online Retail 在线零售交易数据集，使用 Python（pandas / numpy）完成数据清洗，并在此基础上构建 RFM 客户价值分析表，为后续的用户分层、精准营销等分析奠定数据基础。

## 项目结构

```
JupyterProject1/
├── data/                  # 数据目录（预留）
├── models/                # 模型目录（预留）
├── online_retail.csv      # 原始交易数据
├── clean_retail.csv       # 清洗后的交易数据（CSV 格式）
├── clean_retail.xlsx      # 清洗后的交易数据（Excel 格式）
├── customer_table.xlsx    # RFM 客户价值分析表
├── 数据清洗.ipynb          # 数据清洗与 RFM 分析主流程
├── sample.ipynb           # PyCharm 示例 Notebook（可忽略）
└── requirements.txt       # 项目依赖
```

## 数据说明

原始数据 `online_retail.csv` 为在线零售交易明细，主要字段包括：

| 字段 | 说明 |
| --- | --- |
| InvoiceNo | 发票编号，以 `C` 开头的为取消订单 |
| StockCode | 商品编码 |
| Description | 商品描述 |
| Quantity | 购买数量 |
| InvoiceDate | 发票日期 |
| UnitPrice | 商品单价 |
| CustomerID | 客户编号 |
| Country | 客户所在国家 |

## 环境依赖

- Python 3.9
- pandas == 2.3.3
- numpy == 2.0.2

安装依赖：

```bash
pip install -r requirements.txt
```

## 使用方式

1. 安装依赖（见上文）。
2. 确保 `online_retail.csv` 位于项目根目录。
3. 打开并运行 `数据清洗.ipynb`，按顺序执行所有单元格。
4. 运行完成后，项目根目录将生成以下输出文件：
   - `clean_retail.csv` / `clean_retail.xlsx`：清洗后的交易数据
   - `customer_table.xlsx`：RFM 客户价值分析表

## 数据清洗流程

主流程位于 `数据清洗.ipynb`，主要步骤如下：

1. **数据导入**：读取 `online_retail.csv`。
2. **数据检查**：
   - 查看数据形状、列名与数据类型
   - 统计重复行数
   - 统计 InvoiceNo、Country 的去重个数
   - 检查 Quantity、UnitPrice 的负值数量与描述性统计
   - 统计各字段缺失值情况
3. **数据清洗**：
   - 将 `InvoiceDate` 转换为日期类型
   - 删除完全重复的行
   - 删除 `CustomerID`、`InvoiceDate` 缺失的记录
   - 剔除取消订单（`InvoiceNo` 以 `C`/`c` 开头）
   - 剔除 `Quantity <= 0` 与 `UnitPrice <= 0` 的异常记录
   - 新增 `TotalPrice = Quantity * UnitPrice` 字段
4. **清洗结果确认**：输出清洗后的数据形状、客户数、发票数及各类异常值数量。
5. **数据导出**：将清洗结果导出为 CSV 与 Excel 文件。

## RFM 客户价值分析

在清洗后的数据上，按 `CustomerID` 聚合生成 RFM 指标：

| 指标 | 含义 | 计算方式 |
| --- | --- | --- |
| Recency（R） | 最近一次购买距今天数 | 快照日期（最大发票日期 + 1 天）减去该客户最近购买日期 |
| Frequency（F） | 购买频次 | 该客户去重后的发票数量 |
| Monetary（M） | 消费金额 | 该客户 `TotalPrice` 总和 |

结果输出至 `customer_table.xlsx`，可用于后续的客户分层（如重要价值客户、一般客户等）与营销策略制定。

## 说明

- `sample.ipynb` 为 PyCharm 自带的示例 Notebook，与项目无关，可忽略或删除。
- `data/` 与 `models/` 目录为预留目录，当前为空。
